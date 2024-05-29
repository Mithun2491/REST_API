 Set Up Your Environment
 Add rest_framework and myapp to your INSTALLED_APPS in myproject/settings.py
 ***************************************
 INSTALLED_APPS Please remember the following text:

Installed apps = [
...
'rest_framework',
'myapp',
**************************************************
 Define Your Model:
 from django.db import models
    class Item(models.Model ):
    name = models.CharField(max_length=100)
    description = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.name
  ***********************************************
  MAKE MIGRATIONS 
  **********************************
 Create a Serializer:
     from rest_framework import serializers
      from .models import Item

class ItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = Item
        fields = '__all__'
*****************************************************
Create Views and URL Configurations:
from rest_framework import generics
from .models import Item
from .serializers import ItemSerializer

class ItemListCreate(generics.ListCreateAPIView):
    queryset = Item.objects.all()
    serializer_class = ItemSerializer

class ItemRetrieveUpdateDestroy(generics.RetrieveUpdateDestroyAPIView):
    queryset = Item.objects.all()
    serializer_class = ItemSerializer
*****************************************************************
Define the URL patterns in myapp/urls.py:

from django.urls import path
from .views import ItemListCreate, ItemRetrieveUpdateDestroy

urlpatterns = [
    path('items/', ItemListCreate.as_view(), name='item-list-create'),
    path('items/<int:pk>/', ItemRetrieveUpdateDestroy.as_view(), name='item-retrieve-update-destroy'),
]
*****************************************************************************
Include myapp URLs in myproject/urls.py:
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('myapp.urls')),
]

**********************************************************
: Test Your API
python manage.py runserver
******************************************************

List items (GET to /api/items/).
Retrieve an item (GET to /api/items/1/).
Update an item (PUT or PATCH to /api/items/1/).
Delete an item (DELETE to /api/items/1/).

***********************************************************
