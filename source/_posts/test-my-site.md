---
title: fastadmin自定义角色组
date: 2019-01-31 17:36:10
comments: true
tags:
categories: 
- 服务端语言
- php
- fastadmins
---
之前使用fastadmin，自定义角色组，发现编辑/删除/修改等等权限可以直接权限管理中选择，然后用不同的角色登陆，会显示对应的权限按钮。此时有一个新的需求就是增加审核和批量审核，并且也增加对应的权限。


----------

添加按钮
    先不写功能，在对应的位置上加上审核按钮。在顶头位置的批量审核需要得到的样式如下：
    
![clipboard.png](https://image-static.segmentfault.com/105/121/1051217979-5b03e62334d57_articlex)

   

>  原先的创建按钮方法为：
>     {:build_toolbar('refresh,edit,checkall')}
> 有时候不使用buid_toolbar来创建按钮，而是直接使用的
>    
> 
>      <a href="javascript:;" class="btn btn-danger btn-del btn-disabled disabled {:$auth->check('course/clabelong/del')?'':'hide'}"
> title="{:__('Delete')}" ><i class="fa fa-trash"></i>
> {:__('Delete')}</a>

这种方式来创建按钮，这样虽然很直观，但是实际上使用却很不方便，因为这里对于权限的判定可能是有问题的。期待的结果是直接使用build_toolbar方法，设置checkall为批量审核，check为审核方法，对于各个权限组可以直接调用。
修改build_toolbar方法到如下结果：

    function build_toolbar($btns = NULL, $attr = [])
        {
            $auth = \app\admin\library\Auth::instance();
            $controller = str_replace('.', '/', strtolower(think\Request::instance()->controller()));
            $btns = $btns ? $btns : ['refresh', 'add', 'edit', 'del', 'import','checkall'];
            $btns = is_array($btns) ? $btns : explode(',', $btns);
            $index = array_search('delete', $btns);
            if ($index !== FALSE) {
                $btns[$index] = 'del';
            }
            $btnAttr = [
                'refresh' => ['javascript:;', 'btn btn-primary btn-refresh', 'fa fa-refresh', '', __('Refresh')],
                'checkall'=> ['javascript:;', 'btn btn-success btn-checkall btn-disabled disabled', 'fa fa-flag-checkered', ' 批量审核', '批量审核'],
                'add'     => ['javascript:;', 'btn btn-success btn-add', 'fa fa-plus', __('Add'), __('Add')],
                'edit'    => ['javascript:;', 'btn btn-success btn-edit btn-disabled disabled', 'fa fa-pencil', __('Edit'), __('Edit')],
                'del'     => ['javascript:;', 'btn btn-danger btn-del btn-disabled disabled', 'fa fa-trash', __('Delete'), __('Delete')],
                'import'  => ['javascript:;', 'btn btn-danger btn-import', 'fa fa-upload', __('Import'), __('Import')],
            ];
            $btnAttr = array_merge($btnAttr, $attr);
            $html = [];
            foreach ($btns as $k => $v) {
                //如果未定义或没有权限
                if (!isset($btnAttr[$v]) || ($v !== 'refresh' && !$auth->check("{$controller}/{$v}"))) {
                    continue;
                }
                list($href, $class, $icon, $text, $title) = $btnAttr[$v];
                $extend = $v == 'import' ? 'id="btn-import-file" data-url="ajax/upload" data-mimetype="csv,xls,xlsx" data-multiple="false"' : '';
                $html[] = '<a href="' . $href . '" class="' . $class . '" title="' . $title . '" ' . $extend . '><i class="' . $icon . '"></i> ' . $text . '</a>';
            }
            return implode(' ', $html);
        }
                    
具体修改的就是两个地方，第一个是数组$btns，在$btns数组中添加需要的元素，在本例中添加checkall;第二就是修改$btnAttr，根据需要添加一个checkall元素，如下所示：

    

> 'checkall'=> ['javascript:;', 'btn btn-success btn-checkall
> btn-disabled disabled', 'fa fa-flag-checkered', ' 批量审核', '批量审核'],

            
这里有需要特别注意的地方是，最好在类中加上**btn-disabled disabled**两个class,这样可以方便直接获取是否选择。也就是说，在没有选择项的情况下，所有批量操作的按钮都应该是不能点击的。
在修改完buildtoolbar之后在页面上使用应该就是可以直接调用出来了，到此第一步完成。


----------

修改js配置

接下来修改require-table.js文件，因为所有的后台js文件都继承了这个文件，如果用的编辑器或IDE没有历史记录功能那就一定要做好备份。
    首先是配置项，extend项代表每个url对应的方法，这里可以设置成缺省为空，然后在需要用到的js文件中再调用。加上checkall和check的URL，最好命名一致，修改完成的extend如下：
    extend: {
                index_url: '',
                add_url: '',
                edit_url: '',
                del_url: '',
                import_url: '',
                multi_url: '',
                check_url:'',
                checkall_url:'',
                check_classes_url:'',
                dragsort_url: 'ajax/weigh',
            }
只需要按自己的需求把想加的公共方法加上，在config中添加按钮对应的class,比如这里添加：

    checkallbtn: '.btn-checkall',


绑定事件
在bindevent中添加事件如下：

    // 批量审核按钮事件
    $(toolbar).on('click', Table.config.checkallbtn, function () {
        var that = this;
        var ids = Table.api.selectedids(table);
        layer.msg('您正在批量修改审核状态，请选择结果', {
            time: 20000, //20s后自动关闭
            btn: ['通过', '不通过', '取消']
            ,yes: function(index){
                $(this).data({'check':1,'tablename':'classes'});
                Table.api.multi("checkall", ids, table, $(this));
                Layer.close(index);
                  }
            ,btn2: function(index){
                $(this).data({'check':2,'tablename':'classes'});
                Table.api.multi("checkall", ids, table, $(this));
                Layer.close(index);
                 }
            ,btn3: function(index){
                Layer.close(index);
            }
        });
    });

其中Table.config.checkallbtn即之前在config中添加的按钮，其中绑定的类名必须在之前的buildtollbar的对应按钮上有，根据需求，这里设置一个弹窗，以点击通过为例：
鼠标选择几项结果之后进行批量审核，如果选择通过，对应回调yes，此时设置两个数据，一个是对应的表，一个是审核状态。因为主要是classes表会用到审核，默认值设置为classes表，如果其他方法调用，可以在对应的js文件中重写绑定事件；（还有另一种方法，config中添加一项为tablename,然后直接传tablename,将对应的classes修改为Table.config.tablename,每次页面js调用的时候修改config，但是因为大部分页面用不着重写config,因此这里不这么写）
multi方法即批量操作方法，因此这里应该直接调用，在multi方法中有以下的一句：
var url = typeof data.url !== "undefined" ? data.url : (action == "del" ? options.extend.del_url : options.extend.multi_url);

这里要注意的是，在js调用的时候打印url，可能审核方法不在，因为实际上multi方法是绑定到dek_url或者multi_url的，这里有两种方法，一种是把check_url替换上去，一种是直接把multi_url方法绑定到check上，也可以直接加上，但是这个判断就要改。


具体js
进入到控制器对应的js中调用方法，如果写在trait中，每个控制器都会直接继承，如果使用的地方较少，可以直接使用。地址是admin/library/traits/Backend.php
checkall方法如下：

    /**
         *
         * 批量审核
         */
        public function checkall($ids = "")
        {
            $row = $this->model->get($ids);
            if (!$row)
                $this->error(__('No Results were found'));
            $adminIds = $this->getDataLimitAdminIds();
            if (is_array($adminIds)) {
                if (!in_array($row[$this->dataLimitField], $adminIds)) {
                    $this->error(__('You have no permission'));
                }
            }
            if ($this->request->isPost()) {
                $values = $this->request->post()['params'];
                if($values)
                {
                    $res =Db::name($values['tablename'])->where('id','IN',$ids)->update(['check'=>$values['check']]);
                    $this->success();
                }
            }
        }
        


其中$this->request->post()['params']可以获取到刚才在js中传递的数据，这里是用户选择的审核状态和需要更新的表名；之后对表进行操作返回$this->success()就可以了；


添加权限
以上操作顺利完成之后显示的会是没有操作权限；首先进入规则管理，添加刚才新增的方法对应的规则，例如这里添加的是check:

![clipboard.png](https://segmentfault.com/img/bVba37O?w=736&h=333)

只要不以菜单方式显示，然后状态正常就可以了；
之后进入角色组，在角色组对应的权限中勾选，此时就可以加上对应的权限，这是很重要的一步：

    **接下来在index页面中加上验证就行，例如这里是：
    data-operate-check="{:$auth->check('course/clabelong/check')}"
    data-operate-checkall="{:$auth->check('course/clabelong/checkall')}"**

此时再登录不同的用户，例如教师组没有批量审核权限，那么无法显示批量审核按钮。


单个操作权限
除了批量操作，其实还可以单个操作，单个操作和批量操作稍有不同，但是更简单。可以仿照单元格元素事件来写，结果如下：

    'click .btn-checkone': function (e, value, row, index) {
                            e.stopPropagation();
                            e.preventDefault();
                            var table = $(this).closest('table');
                            var options = table.bootstrapTable('getOptions');
                            var ids = row[options.pk];
                            row = $.extend({}, row ? row : {}, {ids: ids});
                            var url = options.extend.check_url;
                            Fast.api.open(Table.api.replaceurl(url, row, table), '审核', $(this).data() || {});
                        },

直接绑定到check_url，在对应方法的js文件中的extend写出check_url的位置，添加对应的方法和视图，就可以直接调用。当然，同样涉及到权限的问题，加了方法之后在index中依然要用data-operate-check="{:$auth->check('course/clabelong/check')}"；同时，不要忘记在require-table.js中添加operate;我添加如下代码：

    if (options.extend.check_url !== '') {
                            buttons.push({
                                name: 'check',
                                icon: 'fa fa-flag-checkered',
                                title: '审核',
                                classname: 'btn btn-xs btn-success btn-checkone',
                                url: options.extend.check_url
                            });
                        }


在require-table中可能还有需要修改的地方，但是关键是按照原先的add.del等写好的方法来参照，基本上就不会出错了。
        