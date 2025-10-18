# Django-notes

## Project Setup:
---
First we need to setup a django project in a virtual environment so that other packages don't conflict with the packeges used in the project.
The following code creates a python virtual environment:
```cmd
python -m venv env
```
We have to activate the environment by running the following command:
```cmd
.\env\Scripts\activate
```
Now we have to install the required packages with pythons package manager called ***pip***.
```cmd
pip install django
pip install pillow
```
To see what packages we have installed we can use the following command:
```cmd
pip freeze
```
After installing all the necessary packages, we have to start a project by writing the following command:
```cmd
django-admin startproject project_name
```
After running the commands above, we can se a project folder with the given project name. We can open that folder in the code editor and start coding.
We can also setup the project further by running the following commands:
```cmd
cd project_name
python manage.py startapp app_name
mkdir templates
code .
```
`code .` will open the project in vscode. Then we have to configure the settings.py file in the project folder.

1. Add `app_name` to `INSTALLED_APP` list:

 ![Add app_name to INSTALLED_APP list](/images/addInstalledApp.jpg)

 2. Add `templates` folder name to the `TEMPLATES` list:

 ![Add app_name to INSTALLED_APP list](/images/addInstalledApp.jpg)

 3. 



