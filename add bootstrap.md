## 安装 Bootstrap 及样式

安装参考网站：
1.https://medium.com/@guilpejon/how-to-install-bootstrap-4-3-in-a-rails-6-app-using-webpack-9eae7a6e2832
2.https://fullstack.qzy.camp/posts/542
感谢上述网站，受益匪浅，这些方案是我在众多方案选择和实践中最成功的。


#### Step 1.  设定。运用webpacker 来监视我们的程式

1. 新建rails程式文件夹
  rails new  文件名
2.  在app/javascript/ 新建文件夹，并设置程式scss文件
   mkdir app/javascript/stylesheets
   touch app/javascript/stylesheets/application.scss
3. 在app/javascript/packs/application.js中与刚建立的文件产生关联
    import '../stylesheets/application'
4. 告诉rails 开始使用wepack。在app / views / layouts / application.html.erb 文件下修改为
   修改前：
-    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
-    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
   
-    修改后：
+  <%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
+  <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>

### Step 2   设置 foreman（非必须选者）  读者说这个可以加快加载网页的时间
1.安装
gem install foreman
2.在创建该程式的根目录下创建文件Procfile.dev   并加入代码
  touch Procfile.dev
  为新建的 Procfile.dev  添加下列为代码
web: bundle exec puma -C config/puma.rb
webpacker: ./bin/webpack-dev-server
3.终端机运行foreman
foreman start -f Procfile.dev

### Step 3   添加 bootrap
1.安装bootstrap及其必需的依赖项插件 jquery和popper
  yarn add bootstrap@4.3.1 jquery popper.js   （4.3.1。 该版本我测试能用为版本数，可根据自己需求选者想要的版本）

2.在文件config/webpack/environment.js。添加代码
...
const webpack = require('webpack')
environment.plugins.append(
  'Provide',
  new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    Popper: ['popper.js', 'default']
  })
)
...
3.在app/javascript/packs新建bootstrap_custom.js 并引入代码
 &  touch app/javascript/packs/bootstrap_custom.js

##### app/javascript/packs/bootstrap_custom.js
import 'bootstrap/js/dist/alert'
import 'bootstrap/js/dist/button'
import 'bootstrap/js/dist/carousel'
import 'bootstrap/js/dist/collapse'
import 'bootstrap/js/dist/dropdown'
import 'bootstrap/js/dist/index'
import 'bootstrap/js/dist/modal'
import 'bootstrap/js/dist/popover'
import 'bootstrap/js/dist/scrollspy'
import 'bootstrap/js/dist/tab'
import 'bootstrap/js/dist/toast'
import 'bootstrap/js/dist/tooltip'
import 'bootstrap/js/dist/util'

4.建立链接
# app/javascript/packs/application.js
import './bootstrap_custom.js'

5.引入样式
# app/javascript/stylesheets/application.scss
@import './bootstrap_custom.scss';

6. 新建scss ，并将样式放入app/javascript/stylesheets/bootstrap_custom.scss

& touch app/javascript/stylesheets/bootstrap_custom.scss

@import '~bootstrap/scss/_functions.scss';
@import '~bootstrap/scss/_variables.scss';
@import '~bootstrap/scss/_mixins.scss';
@import '~bootstrap/scss/_root.scss';
@import '~bootstrap/scss/_reboot.scss';
@import '~bootstrap/scss/_type.scss';
@import '~bootstrap/scss/_alert.scss';
@import '~bootstrap/scss/_badge';
@import '~bootstrap/scss/_breadcrumb';
@import '~bootstrap/scss/_button-group';
@import '~bootstrap/scss/_buttons.scss';
@import '~bootstrap/scss/_card.scss';
@import '~bootstrap/scss/_carousel.scss';
@import '~bootstrap/scss/_close.scss';
@import '~bootstrap/scss/_code.scss';
@import '~bootstrap/scss/_custom-forms.scss';
@import '~bootstrap/scss/_dropdown.scss';
@import '~bootstrap/scss/_forms.scss';
@import '~bootstrap/scss/_grid.scss';
@import '~bootstrap/scss/_images.scss';
@import '~bootstrap/scss/_input-group.scss';
@import '~bootstrap/scss/_jumbotron.scss';
@import '~bootstrap/scss/_list-group.scss';
@import '~bootstrap/scss/_media.scss';
@import '~bootstrap/scss/_modal.scss';
@import '~bootstrap/scss/_nav.scss';
@import '~bootstrap/scss/_navbar.scss';
@import '~bootstrap/scss/_pagination.scss';
@import '~bootstrap/scss/_popover.scss';
@import '~bootstrap/scss/_print.scss';
@import '~bootstrap/scss/_progress.scss';
@import '~bootstrap/scss/_spinners.scss';
@import '~bootstrap/scss/_tables.scss';
@import '~bootstrap/scss/_toasts.scss';
@import '~bootstrap/scss/_tooltip.scss';
@import '~bootstrap/scss/_transitions.scss';
@import '~bootstrap/scss/_utilities.scss';
  
##### 注意事项
1.注意：请勿更改scss导入的顺序（至少是顶部的导入顺序），否则会出现各种错误
2.我们不必node_modules将路径的一部分添加到导入中，因为Webpack已经知道在哪里寻找它们。（该条未理理解）！
##### 记得保存
git add .
git commit -m "add bootstrap to project"

#### Step 4  增加 导航栏
1.新增 app/views/common 资料夹
mkdir app/views/common
2.新增 navbar
touch app/views/common/_navbar.html.erb
填入
<nav class="navbar navbar-expand-lg navbar-light bg-light">

  <a class="navbar-brand" href="/">Job Listing</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">

    </ul>

    <ul class="nav navbar-nav ">
      <% if !current_user %>
      <div class="mx-auto" style="width: 50px;">
       <li><%= link_to("注册", new_user_registration_path) %> </li>
      </div>
       <li><%= link_to("登录", new_user_session_path) %></li>

     <% else %>
       <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown">
            Hi!, <%= current_user.email %>
          <b class="caret"></b>
         </a>
         <ul class="dropdown-menu">
          <li> <%= link_to("Admin Panel", admin_jobs_path) %></li>
          <li class="dropdown-divider"> </li>
         <li> <%= link_to("退出", destroy_user_session_path, method: :delete) %> </li>
        </ul>
       </li>
     <% end %>
    </ul>
  </div>

</nav>

3.新增 footer
touch app/views/common/_footer.html.erb
填入
<footer class="container" style="margin-top: 100px;">
    <p class="text-center">Copyright © 你想要的名称
        <br>Design by yourname
        
    </p>
</footer>

4.修改全域 HTML 样式 application.html.erb
app/views/layouts/application.html.erb
填入
<!DOCTYPE html>
<html>
  <head>
    <title>JobListing1</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <div class="container-fluid">
        <%= render "common/navbar" %>
        <%= yield %>
    </div>

    <%= render "common/footer" %>
  </body>
</html>
5. 产生一个新的空 Hello World 页面 （放在 welcome#index)
rails g controller welcome
touch app/views/welcome/index.html.erb
填入
<h1> Hello World! </h1>

6.将首页指到 welcome 下的 index.html.erb 页面
修改 config/routes.rb，改成以下内容

Rails.application.routes.draw do
  root 'welcome#index'
end

####  保存进度
git add .
git commit -m "add bootstrap html"

## Step 5  增加 flash 功能
1.新增 app/views/common/_flashes.html.erb
touch app/views/common/_flashes.html.erb
填入
<% if flash.any? %>
  <% user_facing_flashes.each do |key, value| %>
    <div class="alert alert-dismissable alert-<%= flash_class(key) %>">
      <button class="close" data-dismiss="alert">×</button>
      <%= value %>
    </div>
  <% end %>
<% end %
2.加入 app/helpers/flashes_helper.rb
touch app/helpers/flashes_helper.rb
添加
module FlashesHelper
  FLASH_CLASSES = { alert: "danger", notice: "success", warning: "warning"}.freeze

  def flash_class(key)
    FLASH_CLASSES.fetch key.to_sym, key
  end

  def user_facing_flashes
    flash.to_hash.slice "alert", "notice","warning" 
  end
end
3.在 application.html.erb 内加入 flash 这个 partial
在 <%= yield %> 前加入 <%= render "common/flashes" %>

app/views/layouts/application.html.erb
  <%= render "common/flashes" %>
  <%= yield %>
    保存进度
    git add .
git commit -m "add bootstrap flash function"
