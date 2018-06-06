---
layout: post
title:      "Rails App with a jQuery Front End"
date:       2018-06-06 13:01:04 +0000
permalink:  rails_app_with_a_jquery_front_end
---


After a month or so break from Ruby on Rails while I set off to wrap my head around the magic that is Javascript, I was excited to start fusing my newly learned JS skills with my trusty ole' Rails knowledge. The JS requirements for this project are mainly focused on making requests to the Rails API, and rendering them on the DOM. 


## Must render at least one index page (index resource - 'list of things') via jQuery and an Active Model Serialization JSON Backend.
Once a User is logged in, on the root page there is a button "Your Groups". Previously this button would direct you to "/groups" , where you would find a list of your groups. Now, with the help of some AJAX, a user's groups are appended right under the "Your Groups" button. Each group is a clickable link that group's show page.

Code:
```
const getGroups = () => {
  $.getJSON('http://localhost:3000/groups', (data) => {
    data.forEach(group => {
      let createdGroup = new Group(group.name, group.id)
      $('#groupsList').append(createdGroup.renderGroup())
    })
  })
};

class Group {
  constructor(name, id){
    this.name = name;
    this.id = id;
  }
  renderGroup() {
    return `<a href="http://localhost:3000/groups/${this.id}"><h5>${this.name}</h5></a>`;
  }
}
```

Active Model Serialzer, makes rendering to JSON a breeze.

```
class GroupSerializer < ActiveModel::Serializer
  attributes :id, :name
  has_many :users
  has_many :lists
end
```

## Must render at least one show page (show resource - 'one specific thing') via jQuery and an Active Model Serialization JSON Backend. The rails API must reveal at least one has-many relationship in the JSON that is then rendered to the page
On a group show page there is a card with the Group Name on top, and two columns, one with the users in the group and the second  lists that are shared by the users. I added a "Next Group" button to show the current user's next group without redirecting to that group show route. Here the AMS has_many came in very handy, as Groups has_many :users and has_many :lists

When the "Next Group" button is clicked, the group name is replaced for the Next group name, all the li's on users and lists are removed and the new ones are appended in their stead.

Code:
```
function showNextGroup(event){
  event.preventDefault();

  $.getJSON(this.href, function(data){

    let name = data.name
    let users = data.users
    let lists = data.lists

    $('#groupName').html(name)

    $('#groupUsers li').remove();
    users.forEach(user => {
      $('#groupUsers').append(`<li class="list-group-item"> ${user.email} </li>`)
    })
    $('#groupLists li').remove();
    lists.forEach(list => {
      $('#groupLists').append(`<li class="list-group-item"><a href="/groups/${list.group_id}/lists/${list.id}"> ${list.name} </a></li>`)
    })
  })
}
```

## Must use your Rails API and a form to create a resource and render the response without a page refresh.
For this requirement a User can create a new comment, and that comment is rendered to the DOM without the page refreshing. 

Code:
```
$(function(){
  $('#new_comment').submit(function(e){
    e.preventDefault();
    let values = $(this).serialize();
    let posting = $.post(this.action, values)
      posting.done(data => {
        let newComment = new Comment(data.id, data.user.email, data.content)
        $("#commentsTable tbody").append(newComment.renderComment());
      })
    $("#comment_content").val(" ");
  })
});

class Comment {
  constructor(id, email, content){
    this.id = id;
    this.email = email;
    this.content = content;
  }
  renderComment() {
    return `<tr><td>${this.email}</td><td>${this.content}</td><th><a href="/comments/${this.id}/edit">edit comment</a></th></tr>`;
  }
}
```

## Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype
For both  Groups and Comments, JS Class notation and constructors were used. 

```
//groups.js

class Group {
  constructor(name, id){
    this.name = name;
    this.id = id;
  }
  renderGroup() {
    return `<a href="http://localhost:3000/groups/${this.id}"><h5>${this.name}</h5></a>`;
  }
}

//comments.js

class Comment {
  constructor(id, email, content){
    this.id = id;
    this.email = email;
    this.content = content;
  }
  renderComment() {
    return `<tr><td>${this.email}</td><td>${this.content}</td><th><a href="/comments/${this.id}/edit">edit comment</a></th></tr>`;
  }
}
```

Each class also has a prototype method which can (and are) called on the JS Models.

## Wrap Up
Coming from the user friendliness of Ruby and Rails, purely JS was certainly a change. So getting to bring that new JS knowledge back to rails was a great feeling. Although, I am not exactly sure what React.js is, I am pumped to find out!
