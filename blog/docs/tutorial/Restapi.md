# Building a REST API in Django

Learn how to create a RESTful API using Django REST Framework (DRF).

## Prerequisites
- Django installed
- Django REST Framework installed:
   pip install djangorestframework

1. **Add DRF to INSTALLED_APPS in settings.py**:

        INSTALLED_APPS = [
            ...,
            'rest_framework',
        ]

2. **Create a Model**:
        
        from django.db import models

        class Post(models.Model):
            title = models.CharField(max_length=100)
            content = models.TextField()

3. **Create a Serializer**:

        from rest_framework import serializers
        from .models import Post

        class PostSerializer(serializers.ModelSerializer):
            class Meta:
                model = Post
                fields = '__all__'

4. **Set Up a View**:

        from rest_framework.views import APIView
        from rest_framework.response import Response
        from .models import Post
        from .serializers import PostSerializer

        class PostAPIView(APIView):
            def get(self, request):
                posts = Post.objects.all()
                serializer = PostSerializer(posts, many=True)
                return Response(serializer.data)

5. **Configure a URL**:

        from django.urls import path
        from .views import PostAPIView

        urlpatterns = [
            path('api/posts/', PostAPIView.as_view(), name='post-list'),
        ]
