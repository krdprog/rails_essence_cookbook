# Пошаговая инструкция, как создать сущность

**Create model:**
```bash
rails g model Project title:string body:text
```

```bash
rake db:migrate
```

**Create controller:**
```bash
rails g controller Projects
```

**Write route in flie /config/routes.rb:**
```ruby
resources :projects
```

**Create files for views in /app/views/projects:**
```bash
touch index.html.erb show.html.erb new.html.erb edit.html.erb
```

**Create actions in controller /app/controllers/projects_controller.rb:**

```ruby
class ProjectsController < ApplicationController
  def index
    @projects = Project.all
  end

  def show
    @project = Project.find(params[:id])
  end

  def new
  end

  def edit
    @project = Project.find(params[:id])
  end

  def create
    @project = Project.new(project_params)
    if @project.valid?
      @project.save
      redirect_to @project
    else
      render action: 'new'
    end
  end

  def update
    @project = Project.find(params[:id])

    if @project.update(project_params)
      redirect_to @project
    else
      render action: 'edit'
    end
  end

  def destroy
    @project = Project.find(params[:id])
    @project.destroy

    redirect_to projects_path
  end

  private

  def project_params
    params.require(:project).permit(:title, :body)
  end
end
```

**Edit files with views in /app/views/projects:**

**index.html.erb**

```ruby
<h1>Проекты</h1>

<%= link_to "Создать новый проект", new_project_path %>

<hr>

<%= 'В текущей ленте нет проектов' if @projects.empty? %>

<% @projects.each do |project| %>
  <h3><%= link_to project.title, project_path(project) %></h3>
  <p><%= link_to "Show project", project_path(project) %> | <%= link_to "Edit project", edit_project_path(project) %> | <%= link_to "Destroy", project_path(project), method: :delete, data: { confirm: 'Действительно удалить?'} %></p>
  <hr>
<% end %>
```


**show.html.erb**

```ruby
<h1><%= @project.title %></h1>

<p><%= @project.body %></p>

<%= link_to "Назад в проекты", projects_path %>
```


**new.html.erb**

```ruby
<h1>Создать проект:</h1>

<%= form_for :project, url: projects_path do |f| %>
  <p>
    <%= f.label :title, 'Заголовок страницы:' %><br />
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body, 'Текст страницы:' %><br />
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit 'Сохранить' %></p>
<% end %>
```


**edit.html.erb**

```ruby
<h1>Редактировать проект:</h1>

<%= form_for :project, url: project_path(@project), method: :patch do |f| %>
  <p>
    <%= f.label :title, 'Заголовок страницы:' %><br />
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body, 'Текст страницы:' %><br />
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit 'Сохранить' %></p>
<% end %>
```
