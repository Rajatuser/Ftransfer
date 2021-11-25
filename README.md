File transfer webapp using django

Technology used : python

Packages and Softwares that are required to be installed: 
1. Virtual environment setup [In any IDE - Pycharm Professional(recommended)]

code:

Templates:

Base.html
  
           <!doctype html>
          <html lang="en">
            <head>
              <!-- Required meta tags -->
              <meta charset="utf-8">
              <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

              <!-- Bootstrap CSS -->
              <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">

              <title>{% block title %}{% endblock %}</title>
            </head>
            <body>
              <nav class="navbar sticky-top navbar-expand-lg navbar-dark bg-dark">
            <a class="navbar-brand" href="{% url 'home' %}">Share-o-File</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
              <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
              <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                  <a class="nav-link" href="{% url 'home' %}">Home <span class="sr-only">(current)</span></a>
                </li>
              </ul>
              <form action="{% url 'search' %}" method="get" class="form-inline my-2 my-lg-0" style="position: absolute; margin-left: 70px;">
                <input class="form-control mr-sm-2" type="search" name="query" placeholder="Search" aria-label="Search">
                <button class="btn btn-outline-danger my-2 my-sm-0" type="submit">Search</button>
              </form>

              <!-- Button trigger modal -->
              {% if request.session.user %}
              <div class="btn-group">
                <button type="button" class="btn btn-secondary dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                  @{{request.session.user}}
                </button>
                <div class="dropdown-menu dropdown-menu-right">
                  <a href="{% url 'profile' request.session.user %}"><button class="dropdown-item" type="button">Profile</button></a>
                  <a href="{% url 'upload_file' request.session.user %}"><button class="dropdown-item" type="button">Upload File</button></a>
                  <a href="{% url 'logout' %}"><button class="dropdown-item" type="button" style="color: #DC3545">Logout</button></a>
                </div>
              </div>
              {% else %}
                <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#loginModal">
                Login
              </button>

              <button type="button" class="btn btn-outline-primary mx-3" data-toggle="modal" data-target="#signupModal">
                Sign Up
              </button>
              {% endif %}

            </div>
          </nav>

          {% if messages %}
            {% for message in messages %}
            <div class="alert alert-{{message.tags}} alert-dismissible fade show" role="alert">
              {{message}}
              <button type="button" class="close" data-dismiss="alert" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
            </div>
            {% endfor %}
          {% endif %}

          <!-- Login Modal Form -->
          <div class="modal fade" id="loginModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="exampleModalLabel">Login</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  <form action="{% url 'login' %}" method="post">
                    {% csrf_token %}
                    <div class="form-group">
                      <label for="exampleInputEmail1">Username</label>
                      <input type="text" class="form-control" name="uname" required id="exampleInputEmail1" aria-describedby="emailHelp">
                    </div>
                    <div class="form-group">
                      <label for="exampleInputPassword1">Password</label>
                      <input type="password" class="form-control" name="pwd" required id="exampleInputPassword1">
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                      <button type="submit" class="btn btn-primary">Login</button>
                    </div>
                  </form>
                </div>

              </div>
            </div>
          </div>


          <!-- Signup Modal Form -->
          <div class="modal fade" id="signupModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="exampleModalLabel">Sign Up</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
                <div class="modal-body">

                  <form action="{% url 'home' %}" method="post">
                    {% csrf_token %}
                    <div class="form-group">
                      <label for="exampleInputEmail1">Username</label>
                      <input type="text" class="form-control" name="uname" required id="exampleInputText">
                    </div>
                    <div class="form-group">
                      <label for="exampleInputPassword1">Password</label>
                      <input type="password" class="form-control" name="pwd1" required id="exampleInputPassword1">
                    </div>
                    <div class="form-group">
                      <label for="exampleInputPassword2">Confirm Password</label>
                      <input type="password" class="form-control" name="pwd2" required id="exampleInputPassword2">
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                      <button type="submit" class="btn btn-primary">Sign Up</button>
                    </div>
                  </form>
                </div>

              </div>
            </div>
          </div>
              {% block body %}{% endblock %}


              <!-- Option 1: jQuery and Bootstrap Bundle (includes Popper) -->
              <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
              <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>

            </body>
</html>

home.html

                {% extends 'main/base.html' %}
            {% block title %}Home Page{% endblock %}
            {% block body %}
            <!--Posts -->
            {% for post in posts %}
            <div class="container my-4 alert alert-success"> 
                <div class="card">
                    <h5 class="card-header">By <a style="text-decoration:none; color:#DC3545;" href="{% url 'profile' post.user.username %}">{{post.user}}</a></h5>
                    <div class="card-body">
                        <h5 class="card-title">{{post.title}}</h5>
                        <p class="card-text">{{post.desc}}</p>

                        {% if request.session.user %}
                            <a href="{{post.file_field.url}}" class="btn btn-warning" target="_blank">View</a>
                            <a href="{{post.file_field.url}}" class="btn btn-info mx-4" download>Download</a>  
                        {% else %}
                            <a href="#">You Have To Login Before Download this File.</a>
                        {% endif %}
                    </div>
                </div>
            </div>
            {% endfor %}
            {% endfor %}


Profile.html

                        {% extends 'main/base.html' %}
                        {% load static }
                        {% block title %}Profile{% endblock %}

                        {% block body%}
                            <div class="container alert alert-success my-2">
                                {% if user_data.username == request.session.user %}
                                    <h2>Welcome <span style="color:#DC3545;">{{user_data.username}}</span> !</h2>
                                {% else %}
                                    <h2 style="color:#DC3545;">{{user_data.username}}</h2>
                                {% endif %}
                            </div>

                            <div class="container alert alert-success my-2">
                                {% if user_posts %}
                                    {% for post in user_posts%}
                                    <div class="card">
                                        <div class="card-body">
                                            <h2>{{post.title}}</h2>
                                            <p>{{post.desc}}</p>
                                            <a href="{{MEDIA_URL}}/{{post.file_field.url}}" target="_blank" class="btn btn-outline-warning">View</a>
                                            <a href="{{MEDIA_URL}}/{{post.file_field.url}}" download class="btn btn-outline-info">Download</a>
                                            {% if user_data.username == request.session.user %}
                                                <a href="{% url 'delete' post.id %}" class="btn btn-outline-danger" style="float:right;">Delete</a>
                                            {% endif %}
                                        </div>
                                    </div>

                                    {% endfor %}
                                {% else %}
                                    <div class="container">
                                        <h2>No Posts Yet.</h2>
                                    </div>
                                {% endif %}
                            </div>
                        {% endblock %}


search.html


              {% extends 'main/base.html' %}
              {% block title %}Search{% endblock %}

              {% block body%}

                  <div class="container alert alert-success my-2">
                      <h2>Search Results For: <span style="color: #DC3545;">{{query}}</span></h2>
                  </div>


                  <div class="container">
                      <div class="row">
                          <div class="col-3">
                              <div class="nav flex-column nav-pills" id="v-pills-tab" role="tablist" aria-orientation="vertical">
                                  <a class="nav-link active" id="v-pills-home-tab" data-toggle="pill" href="#v-pills-home" role="tab" aria-controls="v-pills-home" aria-selected="true">Users({{search_users.count}})</a>
                                  <a class="nav-link" id="v-pills-profile-tab" data-toggle="pill" href="#v-pills-profile" role="tab" aria-controls="v-pills-profile" aria-selected="false">Posts({{search_result.count}})</a>
                              </div>
                          </div>
                          <div class="col-9">
                              <div class="tab-content" id="v-pills-tabContent">
                                  <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
                                      {% if search_users %}
                                          {% for user in search_users %}
                                          <div class="card">
                                              <div class="card-body">
                                                  <a href="{% url 'profile' user.username %}"><h2>{{user.username}}</h2></a>
                                              </div>
                                          </div>
                                          {% endfor %}
                                      {% else %}
                                          <h2>No Results Found.</h2>
                                      {% endif %}
                                  </div>

                                  <div class="tab-pane fade" id="v-pills-profile" role="tabpanel" aria-labelledby="v-pills-profile-tab">
                                      {% if search_result %}
                                      {% for post in search_result%}
                                      <div class="card">
                                          <div class="card-body">
                                              <h2>{{post.title}}</h2>
                                              <p>{{post.desc}}</p>
                                              <a href="{{MEDIA_URL}}/{{post.file_field.url}}" target="_blank" class="btn btn-outline-warning">View</a>
                                              <a href="{{MEDIA_URL}}/{{post.file_field.url}}" download class="btn btn-outline-info">Download</a>
                                          </div>
                                      </div>

                                      {% endfor %}
                                      {% else %}
                                          <h2>No Results Found.</h2>
                                      {% endif %}
                                  </div>
                              </div>
                          </div>
                      </div>
                  </div>
              {% endblock %}

upload_file.html


                {% extends 'main/base.html' %}

                {% block title %}Upload File{% endblock %}

                {% block body %}

                <div class="container my-4 alert alert-success"> 
                    <div class="container">
                        <form action="{% url 'upload_file' request.session.user %}" method="post" enctype="multipart/form-data">
                            {% csrf_token %}
                            <br>
                            <div class="form-group">
                                <input type="file" name="filename" id="filechoose" required>
                            </div>

                            <div class="form-group">
                                <label for="exampleFormControlInput1" class="display-4">Title</label>
                                <input type="text" class="form-control" name="title" id="exampleFormControlInput1" required>
                            </div>


                            <div class="form-group">
                                <label for="exampleFormControlTextarea1" class="display-4">Description</label>
                                <textarea class="form-control" name="desc" id="exampleFormControlTextarea1" rows="10" required></textarea>
                            </div>

                            <input type="submit" value="Upload" class="btn btn-outline-primary">
                        </form>
                    </div>
                </div>

                {% endblock%}

urls.py(app)

              from django.urls import path
              from . import views

              urlpatterns = [
                  path('', views.HomePageView.as_view(), name='home'),
                  path('login/', views.LoginView.as_view(), name='login'),
                  path('logout/', views.LogoutView.as_view(), name='logout'),
                  path('<str:user_name>/upload_file', views.UploadView.as_view(), name='upload_file'),
                  path('profile/<str:user_name>', views.ProfileView.as_view(), name='profile'),
                  path('delete/<int:post_id>', views.DeleteView.as_view(), name='delete'),
                  path('search/', views.SearchView.as_view(), name='search')
              ]


url.py


              """Transferapp URL Configuration

              The `urlpatterns` list routes URLs to views. For more information please see:
                  https://docs.djangoproject.com/en/3.2/topics/http/urls/
              Examples:
              Function views
                  1. Add an import:  from my_app import views
                  2. Add a URL to urlpatterns:  path('', views.home, name='home')
              Class-based views
                  1. Add an import:  from other_app.views import Home
                  2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
              Including another URLconf
                  1. Import the include() function: from django.urls import include, path
                  2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
              """
              from django.contrib import admin
              from django.urls import path, include

              from django.conf import settings
              from django.conf.urls.static import static

              urlpatterns = [
                  path('admin/', admin.site.urls),
                  path('', include('transfer.urls'))
              ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)


views.py


                    from django.shortcuts import render, redirect
                    from django.views.generic.list import ListView
                    from .models import User, Post
                    from django.http import HttpResponse
                    from django.contrib import messages

                    # Create your views here.
                    class HomePageView(ListView):
                        def get(self, request):
                            '''
                            If user request get method in url direct than reach home page.
                            '''
                            all_posts = Post.objects.all().order_by('-id')
                            param = {'posts':all_posts}
                            return render(request, 'home.html', param)

                        def post(self, request):
                            '''
                            Create account system
                            '''
                            user_name = request.POST['uname']
                            pwd1 = request.POST['pwd1']
                            pwd2 = request.POST['pwd2']
                            print(user_name)
                            print(pwd1)
                            print(pwd2)
                            if pwd1 == pwd2:
                                add_user = User(username=user_name, password=pwd1)
                                add_user.save()
                                messages.success(request, 'Account has been created successfully.')
                                return redirect('home')
                            else:
                                messages.warning(request, 'Passwords are not same.')
                                return redirect('home')




                    # Create Upload File System
                    class UploadView(ListView):
                        def get(self, request, user_name):
                            return render(request, 'upload_file.html')


                        def post(self, request, user_name):
                            filename = request.FILES['filename']
                            title = request.POST['title']
                            desc = request.POST['desc']

                            user_obj = User.objects.get(username=user_name)
                            upload_post = Post(user=user_obj, title=title, file_field=filename, desc=desc)
                            upload_post.save()
                            messages.success(request, 'Your Post has been uploaded successfully.')
                            return render(request, 'upload_file.html')



                    # View User Profile
                    class ProfileView(ListView):
                        def get(self, request, user_name):
                            user_obj = User.objects.get(username=user_name)
                            user_posts = user_obj.post_set.all().order_by('-id')
                            param = {'user_data':user_obj, 'user_posts': user_posts}
                            return render(request, 'profile.html', param)


                    # Post Delete View

                    class DeleteView(ListView):
                        model = Post
                        def get(self, request, post_id):
                            user = request.session['user']
                            delete_post = self.model.objects.get(id=post_id)
                            delete_post.delete()
                            messages.success(request, 'Your post has been deleted successfully.')
                            return redirect(f'/profile/{user}')


                    # Search View
                    class SearchView(ListView):
                        def get(self, request):
                            query = request.GET['query']
                            search_users = User.objects.filter(username__icontains=query)
                            search_title = Post.objects.filter(title__icontains = query)
                            search_desc = Post.objects.filter(desc__icontains = query)
                            search_result = search_title.union(search_desc)
                            param = {'query':query, 'search_result':search_result, 'search_users':search_users}
                            return render(request, 'search.html', param)







                    # Login System
                    class LoginView(ListView):
                        def get(self, request):
                            return redirect('home')

                        def post(self, request):
                            user_name = request.POST['uname']
                            pwd = request.POST['pwd']

                            user_exists = User.objects.filter(username=user_name, password=pwd).exists()
                            if user_exists:
                                request.session['user'] = user_name
                                messages.success(request, 'You are logged in successfully.')
                                return redirect('home')
                            else:
                                messages.warning(request, 'Invalid Username or Password.')
                                return redirect('home')
                            return redirect('home')

                    class LogoutView(ListView):
                        def get(self, request):
                            try:
                                del request.session['user']
                            except:
                                return redirect('home')
                            return redirect('home')




