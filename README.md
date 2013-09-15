tt1
===

practice session 1

first test.
no code yet
encoded semicolon in urls, %3B
python ckan semicolon
html semicolon for description?
yum search pylons
then yum install the package 


How to Add Custom Fields to CKAN 2.0

Note: This method works, but is surely not the most efficient method – it’s just the only way I could get it to work. Edits from core CKAN contributors appreciated.

In order to add custom fields, you need to create and customize an extension. (Relax! This seems intimidating, but once it’s installed, this is pretty easy.)
Generate setup.py file

SSH into your running CKAN instance as normal, e.g.

ssh -i your.pem ubuntu@XXX-XXX-XXX-XXX.compute-1.amazonaws.com

Change to your pyenv directory, e.g.

cd ~/public_html/yoursite.com/pyenv

Activate pyenv, e.g.

. pyenv/bin/activate

Run this command to generate the setup wizard, replacing MYEXTENSION with what you’d like to name yours. Answer the questions as the wizard walks you through.

paster create -t ckanext ckanext-MYEXTENSION

This will create a new directory in the current directory named ckanext-MYEXTENSION. Change into this directory:

cd ckanext-MYEXTENSION

Open the setup.py file:

sudo nano setup.py

Copy its contents to a local file.
Put extension on Github
Create Github repo

Sign in to Github.com. Create a new repository.
Create local git repo

Create a new directory on your local machine, e.g.

mkdir ckanext-MYEXTENSION

Change into this directory:

cd ckanext-MYEXTENSION

Initiatilize a git repo:

git init

Create a README file to get the repo going:

touch README

Add the README to the repo:

git add .
git commit -am "First commit"
git push

NOTE: If you already pushed an initial README to your Github repo and are blocked here, try:

git push --force

Create extension files

You can see an example of these files at: github.com/MarinaMartin/USMetadata and github.com/okfn/ckan/tree/release-v2.0/ckanext/example_idatasetform
Top-level directory

In your MAIN (top-level) git repo directory on your local machine, e.g. ckanext-MYEXTENSION, create a file named “setup.py” and paste the contents of the setup.py generated earlier.

sudo nano setup.py

Also create a blank __init__.py file:

touch __init__.py

Now create a new directory called ckanext:

mkdir ckanext

Switch to this directory:

cd ckanext

ckanext directory

Create a new __init__.py file containing the following:

try:
  import pkg_resources
  pkg_resources.declare_namespace(__name__)
except ImportError:
  import pkgutil
  __path__ = pkgutil.extend_path(__path__, __name__)

Create a directory for your extension and change to it:

mkdir MYEXTENSION
cd MYEXTENSION

MYEXTENSION directory

Create a blank __init__.py and plugin.py file:

touch __init__.py
touch plugin.py

Create a templates directory and change into it:

mkdir templates

templates directory

Create a package directory and change into it:

mkdir package
cd package

package directory

Create a blank read.html file:

touch read.html

Create a snippets directory and change into it:

mkdir snippets
cd snippets

snippets directory

Create two blank files:

touch package_basic_fields.html
touch package_metadata_fields.html

Customize extension fields

Update the following files with the contents of github.com/MarinaMartin/USMetadata

ckanext/MYEXTENSION/plugin.py
ckanext/MYEXTENSION/templates/package/read.html
ckanext/MYEXTENSION/templates/package/snippets/package_basic_fields.html
ckanext/MYEXTENSION/templates/package/snippets/package_metadata_fields.html

Customize these files for the needs of your custom fields.
Update setup.py

Modify the setup.py file in your top-level directory. Under where it says Add Plugins here, uncomment and update:

[ckan.plugins]
# Add plugins here, eg
MYEXTENSION=ckanext.MYEXTENSION.plugin:YOURPLUGIN
"""

Replace MYEXTENSION with the exact same value as your MYEXTENSION directory name from above. Replace YOURPLUGIN with the name of the plugin class inside plugin.py, e.g.

usmetadata=ckanext.usmetadata.plugin:USMetadataPlugin

Push changes to git

git add .
git commit -am "Pushing extension"
git push

Install on your CKAN server

Make sure you are SSHed into your CKAN environment for this step!

sudo pip install -e git+https://github.com/GITUSERNAME/TOPLEVELGITFOLDER#egg=MYEXTENSION

e.g.

sudo pip install -e git+https://github.com/MarinaMartin/USMetadata#egg=usmetadata

Now edit your development.ini and add the extension to your list of plugins:

sudo nano development.ini
ckan.plugins = MYEXTENSION

e.g.

ckan.plugins = stats json_preview recline_preview datastore usmetadata

Restart your production server if needed:

sudo service apache2 restart

Now your custom fields should appear on your dataset packages!

