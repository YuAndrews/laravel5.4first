# laravel5.4first
use the route and laracel json web token
打开app\Providers\AppServiceProvider.PHP，在boot方法中添加如下内容
public function boot()
{
    //sql调试
    $sql_debug = config('database.sql_debug');
    if ($sql_debug) {
        DB::listen(function ($sql) {
            foreach ($sql->bindings as $i => $binding) {
                if ($binding instanceof \DateTime) {
                    $sql->bindings[$i] = $binding->format('\'Y-m-d H:i:s\'');
                } else {
                    if (is_string($binding)) {
                        $sql->bindings[$i] = "'$binding'";
                    }
                }
            }
            $query = str_replace(array('%', '?'), array('%%', '%s'), $sql->sql);
            $query = vsprintf($query, $sql->bindings);
            print_r($query);
            echo '<br />';
        });
    }
}

在app/config/database.php 添加
'sql_debug' => 0, //为1输出所有执行的sql语句


或者在app/helper.php函数添加
 function getLastSql() {
    DB::listen(function ($sql) {
        foreach ($sql->bindings as $i => $binding) {
            if ($binding instanceof \DateTime) {
                $sql->bindings[$i] = $binding->format('\'Y-m-d H:i:s\'');
            } else {
                if (is_string($binding)) {
                    $sql->bindings[$i] = "'$binding'";
                }
            }
        }
        $query = str_replace(array('%', '?'), array('%%', '%s'), $sql->sql);
        $query = vsprintf($query, $sql->bindings);
        print_r($query);
        echo '<br />';
    });
}
在数据库操作语句之前调用即可打印
