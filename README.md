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
    @project = Project.new
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
      render :new
    end
  end

  def update
    @project = Project.find(params[:id])

    if @project.update(project_params)
      redirect_to @project
    else
      render :edit
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
<%= link_to "Создать новый проект", new_project_path %>
<hr>

<h1>Проекты</h1>

<%= 'В текущей ленте нет проектов' if @projects.empty? %>

<% @projects.each do |project| %>
  <h3><%= link_to project.title, project_path(project) %></h3>
<% end %>
```


**show.html.erb**

```ruby
<%= link_to "Все проекты", projects_path %>
|
<%= link_to "Редактировать проект", edit_project_path(@project) %>
|
<%= link_to "Удалить проект", project_path(@project), method: :delete, data: { confirm: 'Действительно удалить?'} %>
<hr>

<h1><%= @project.title %></h1>

<p><%= @project.body %></p>
```


**new.html.erb**

```ruby
<h1>Создать проект:</h1>

<%= render 'form' %>
```


**edit.html.erb**

```ruby
<h1>Редактировать проект:</h1>

<%= render 'form' %>
```
**_form.html.erb**

```ruby
<%= form_for @project do |f| %>
  <p>
    <%= f.label :title, 'Заголовок страницы:' %><br />
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body, 'Текст страницы:' %><br />
    <%= f.text_area :body %>
  </p>

  <p><%= f.submit 'Сохранить' %> | <%= link_to "Отмена", projects_path %></p>
<% end %>
```