Django-quicky
==============

A collection of toys to skip the forplay with Django and go straight to the point: url and view decorators.

You will fall in love with it if you ever had the fantasm you could do:

    @url('/user/\d+')
    @view(render_to='user.html'):
    def user_view(request, id)
        ...
        return {'users': users}


    @user_view.ajax(render_to='json')
    def ajax_user_view(request, id, context):
        ...
        return context

And yes, this is alpha software, but you already want it, so don't fight it !


Url decorators
===============

If you like micro frameworks like <a href="http://bottlepy.org/docs/dev/">bottle</a>, you probably miss the very easy way to declare a route.

Shhh, baby. Talk no more:

    from django_quicky import routing

    url, urlpatterns = routing()


    @url('/any/regex/django/accepts')
    def an_ordinary_view(request):
        ...


    @url('/you/can/stack/routing')
    @url('/any/regex/django/accepts')
    def an_ordinary_view(request):
        ...


Just declare your routing in the view. And use your view file in `URL_ROOT` or any `include()` like you would do with `urls.py`.

Just remember, the order you declare you routing matters as much as it does in urls.py.


View decorators
===============

Rendering template and json bore you to death ? Just say the word:


    from django_quicky import view

    @view(render_to='template.html')
    def an_ordinary_view(request):
        return {'stuff': stuff}


    @view(render_to='json')
    def an_json_view(request):
        return {'stuff': stuff}


For the first one, the returned dictionary will be used as a context to render the template. For the second one, it will be serialised to JSON.

But that's not all. You can also declare alternative rendering:


    from django_quicky import view

    @view(render_to='template.html')
    def common_views(request):
        return {'stuff': stuff}

    @common_views.post()
    def post_view(request, context):
        # do more stuff
        return context

    @common_views.ajax(render_to='json')
    def json_view(request, context):
        return context

The first view will be rendered as-is if it receives a normal GET request. The second vew will be rendered only for POST requests, but will be passed the result of the first view before. The second vew will be rendered only for AJAX requests, and as JSON, but will be passed the result of the first view before.

Just remember that alternative views must accept `context` as a parameter, because they will always receive the result of the main view.