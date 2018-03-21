---
layout: post
title:      "Rails Task Manager"
date:       2018-03-21 09:34:31 +0000
permalink:  rails_task_manager
---


For my Rails Portfolio Project , I decided to go with one of the suggested ideas, a group task management app.  I actually had a really fun time with this project and I felt that I was able to flex a lot of the things I learned throughout the entire Rails section.

For this post I am going to breakdown many of the  requirements for the project and discuss the process by which they were accomplished.

# Models
One of the requirements for the project was that the models include a `has_many`, `belongs_to`, and a `has_many :through` relationship.

The app has five models in total:

**Group**
 
 ` has_many :group_users`
  
`has_many :users, through: :group_users`
  
`has_many :lists`

**User** 

  `has_many :tasks`
 
  `has_many :lists, through: :tasks`
 
  `has_many :group_users`
 
  `has_many :groups, through: :group_users`
	

**Group User (Join Table)**
	 
`belongs_to :group`
    
`belongs_to :user`


**List** 

  `has_many :tasks`

  `has_many :users, through: :tasks`

  `belongs_to :group`

**Task (Join Table)**

  `belongs_to :user`

  `belongs_to :list`
	

# ActiveRecord scope method
For this I wrote a method `#incomplete` that is called on an instance of a task. 

```
def self.incomplete
  where("status = 0")
end
```

Although there are many ways to accomplish a complete/incomplete status on an object, I decided to go with a 0 and 1 model. A task is instantiated with a default value of 0, i.e. incomplete. When a task is completed it's status is changed to 1.

This method is useful for a user to see all of their unaccomplished tasks, like so:

`current_user.tasks.incomplete`

# The Nested Form
For the app, the nested form is a List form that also writes to the Task model. A user can create a new (instance of a ) list, and simultaneously create a new (instance of a) task.

The custom attribute writer is defined in the List class:

```
 def tasks_attributes=(tasks_attributes)
    tasks_attributes.values.each do |task_attribute|
      if task_attribute[:name].present?
        task = self.tasks.build(name: task_attribute[:name])
      end
    end
  end
```

An issue that I ran into with my initial method was using a `find_or_create_by` for the instance of a task. I realized that was an issue when I attempted to create a new task with the same name as another task in the database, and the first instance with the same name was moved to this newer list. 

I realized that each task needs to be a new instance of Task, so me and you can both put "milk" on our respective shopping lists.

# Nested Resources
Due to lists being shared by a group, it made a lot of sense to nest the lists within groups.

```
  resources :groups, only: [:new, :create, :edit, :update, :destroy] do
    resources :lists, only: [:new, :create, :edit, :update, :show, :destroy]
  end
```

# Keeping it DRY with partials
A textbook usage of a partial is for a new and edit forms that are  creating or editing respectively the same fields. And those partials were certainly utilized for all the forms on this app.

One of the main views of this application is the List show view, which is essentially a table, that shows the task name, to who the task is assigned and its status(Done! or Incomplete). 

Additionally, there are **two** forms on the page: one to assign the task to a user and one to mark the task completed. As you can imagine all the information from this table, as well as those forms were making for a fairly clogged view. 

So, away went the forms into partials, and we are left with just a clean looking table:

```
<h1 class="text-center"><%= @list.name %></h1>

<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th scope="col">Task Name</th>
      <th scope="col">Assigned To</th>
      <th scope="col">Status</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <% @list.tasks.each do |task|%>
    <tr>
      <th> <%=task.name%> </th>
      <td> <%= render "tasks/task_assignment", :task => task %> </td>
      <td> <%= task.task_status %></td>
      <td><%= render "tasks/task_status", :task => task %></td>
    </tr>
    <% end %>
  </tbody>
</table>
```

# Authentication
For this app, I used Devise to handle all user signup, login/out validations. I essentially used it right out of the box, not changing any of the settings besides for adding OmniAuth Google. 

# In conclusion
I had a blast making this app.

At times it was challanging and frustrating, but here and there it made me feel like a coding wizard, so that was pretty cool.

And now, it's time to make some room in my coding tool box for JS.




