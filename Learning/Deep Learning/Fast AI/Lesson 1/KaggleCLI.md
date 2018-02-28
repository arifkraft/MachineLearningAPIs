# Kaggle CLI
Kaggle-CLI is a command line tool that will let you download Kaggle data and submit entries from the command line. 
This is particularly useful when working on your AWS/GCP instance. To get started with it, first ssh into your instance
<br>
Then run:
~~~
pip install kaggle-cli 
kg config -g -u `username` -p `password` -c `competition`
~~~

Before you can download the data, you must go to the Kaggle website through the browser and accept the rules for the competition.
You will get an error if you try to download without having accepted the rules.
<br>
Now you are ready to run:
~~~
kg download
~~~
You will need to install and use unzip to unzip.
~~~
sudo apt install unzip
unzip train.zip
unzip test.zip
~~~

Now you're ready to set up your directory structure with sample, classes, and valid directories and sub-directories as 
needed to get the data working with the lesson 1 notebook.
<br>
To unzip 7zip files you need to do the following
```python
sudo apt-get install p7zip-full

7za x myfile.tar.7z
tar -xvf myfile.tar
```
