---
title: 重新实现monthly步骤
date: 2019-03-25 09:41:10
categories: 
- 服务端语言
- python
---
# 1 bi新建已收、已付、应收、应付表
```
CREATE TABLE `order_product_set_pay`(
`id` int(10) unsigned NOT NULL,
`order_product_id` int(10) unsigned NOT NULL,
`parent_order_product_id` int(10) unsigned DEFAULT '0' COMMENT 'parent order_product_id',
`has_settlement` decimal(10,2) NOT NULL DEFAULT '0.00',
`should_settlement` decimal(10,2) NOT NULL DEFAULT '0.00',
`has_pay` decimal(10,2) NOT NULL DEFAULT '0.00',
`should_pay` decimal(10,2) NOT NULL DEFAULT '0.00',
`settlement_pay_time` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
`pay_pay_status` varchar(255) NOT NULL DEFAULT 'Pending',
`pay_pay_time` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


alter table `order_product_set_pay` drop primary key;
alter table `order_product_set_pay` add `settlement_id` int(10) unsigned NOT NULL;
```
# 2 从bi表中查询数据
1 已收
（总收）/比例
比例 = 

# mysql查询优化
**优化结果**
```
 SELECT
                    op.order_product_id,
                    op.parent_order_product_id,
                    op.product_retail,
                    op.product_discount,
                    op.insurance_retail,
                    op.order_id,
                    op.product_cost,
                    opi.`bill_status` AS bill_status,
                    fs.settlement_id,
                    fs.type,
                    fs.`status`,
                    fs.paid_usd_amount AS should_usd,
                    fs.updated_at AS modify_time,
                    (
                        SELECT
                            sum((
                                                CASE 
                                                WHEN (cu.`code`='CNY') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"CNY":', - 1), 6), 2)
                                                WHEN (cu.`code`='GBP') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"GBP":', - 1), 6), 2)
                                                WHEN (cu.`code`='AUD') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"AUD":', - 1), 6), 2)
                                                WHEN (cu.`code`='NZD') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"NZD":', - 1), 6), 2)
                                                WHEN (cu.`code`='EUR') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"EUR":', - 1), 6), 2)
                                                WHEN (cu.`code`='CAD') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"CAD":', - 1), 6), 2)
                                                WHEN (cu.`code`='SUR') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"SUR":', - 1), 6), 2)
                                                WHEN (cu.`code`='JPY') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"JPY":', - 1), 6), 2)
                                                WHEN (cu.`code`='ZAR') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"ZAR":', - 1), 6), 2)
                                                WHEN (cu.`code`='NOK') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"NOK":', - 1), 6), 2)
                                                WHEN (cu.`code`='PHP') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"PHP":', - 1), 6), 2)
                                                WHEN (cu.`code`='SEK') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"SEK":', - 1), 6), 2)
                                                WHEN (cu.`code`='CHF') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"CHF":', - 1), 6), 2)
                                                WHEN (cu.`code`='DKK') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"DKK":', - 1), 6), 2)
                                                WHEN (cu.`code`='HKD') THEN
                                                    ROUND((opii.`amount`)/LEFT(SUBSTRING_INDEX(o.rate_info, '"HKD":', - 1), 6), 2)
                                                WHEN (cu.`code`='USD') THEN  opii.`amount`
                                                END
                                                )) has_paid
                        FROM
                            `bi`.`order_product_item` opii
                        LEFT JOIN `bi`.`order` AS o ON opii.order_id= o.order_id    
                        LEFT JOIN `ori_new`.`financial_bill` AS fb ON opii.bill_id= fb.id
                                                LEFT JOIN `bi`.`order_product` AS opp ON opii.order_product_id= opp.order_product_id
                                                LEFT JOIN `ori_new`.`tff_provider` tp on opp.provider_id=tp.provider_id
                                                LEFT JOIN `bi`.`currency` cu on tp.currency_id=cu.currency_id
                        WHERE
                            opii.order_product_id = op.order_product_id
                        AND (
                            opii.`bill_status` = 'Finished'
                            OR opii.`bill_status` = 'Paid'
                        )
                    ) has_paid,
                    (
                        SELECT
                            fb.updated_at
                        FROM
                            `ori_new`.`financial_bill` fb
                        LEFT JOIN `bi`.`order_product_item` opii  ON opii.bill_id= fb.id
                        WHERE
                            opii.order_product_id = op.order_product_id
                        AND opii.`bill_status` = 'Paid'
                        limit 1
                    ) `pay_pay_time`,
                    (
                        SELECT
                            sum(opa.product_retail) AS total_retail
                        FROM
                            `bi`.`order_product` opa
                        WHERE
                            opa.order_id = op.order_id
                        AND opa.product_line != 'combine'
                    ) total_retail,
                    (
                        SELECT
                            sum(opa.insurance_retail) AS insurance_total
                        FROM
                            `bi`.`order_product` opa
                        WHERE
                            opa.order_id = op.order_id
                        AND opa.product_line != 'combine'
                    ) insurance_total,
                    (
                        SELECT
                            count(opa.order_product_id) AS op_num
                        FROM
                            `bi`.`order_product` opa
                        WHERE
                            opa.order_id = op.order_id
                    ) op_num
                FROM
                    `bi`.`order_product` op
                LEFT JOIN `bi`.`order_product_item` opi ON op.order_product_id = opi.order_product_id
                LEFT JOIN `ori_new`.financial_settlement AS fs ON fs.order_id = op.order_id
                LEFT JOIN `ori_new`.financial_payment_method AS p ON fs.payment_method_id = p.payment_method_id
                WHERE
                    op.order_product_id in ({0})
                AND op.product_line != 'combine'
```


# 用过的命令
```
sudo awk -F '[ ;]+' '{if($2>2) print $1,$2}' test.txt | awk  '{print $1}' |xargs  |tr "\n" " "|sed 's/ /,/g'
sudo sed -i 's/api.bi.tff.com/127.0.0.1:5003/g' `find ./dist/ -name '*js'`
```
