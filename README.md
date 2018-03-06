# ColabForModelling
How to use free GPU from Colab for statistical modelling and save the model object or output in google drive directly

# Working with Colab to use google drive to store the output file


```python
import tensorflow as tf
tf.test.gpu_device_name()
```


```python
! pip install psutil
import psutil
```


```python
!pip install -U -q PyDrive
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials

auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)
```


```python
file1 = drive.CreateFile({'id':'1ZWP7H71JWGjofIZXXXXXXXXX'}) 
file1.GetContentFile('example_file')
```


```python
file = open('example_file', 'r')
content = file.readlines()
```


```python
avira= [ x.split() for x in content]
```


```python
avira1= [ x[1:-1] for x in avira] # we are applying elimination of first and last element of each row(element) in the list
```


```python
def getvalue(x,a):
    if a in x.keys():
        return x[a]
    else:
        return 0

def to_getValue(x):
    return getvalue(x,1)
```


```python
kp_mean = {}
kp_var = {}

for i in range(max_col):
    a=list(map(lambda x : getvalue(x,i+1),avira2))
    kp_mean[i+1] = mean(a)
    kp_var[i+1] = variance(a)
```


```python
# Create GoogleDriveFile instance with title '.
file2 = drive.CreateFile({'title': 'kp'})     # note this will create a new file on google drive 
file2.Upload() # Upload the file.
print('title: %s, id: %s' % (file2['title'], file2['id']))
```


```python
file2 = drive.CreateFile({'id':'1xtKWAHQ1gf6Lw9wrAXXXXX'}) 
file2.GetContentFile('kp')
```


```python
file = open('kp', 'w')
```


```python
for k in kp_mean.keys():
    file.write('-1 key:{} mean:{} varience:{} \n'.format(k,kp_mean[k],kp_var[k]))
file2.Upload() # uploading or writing on the empty file we created in the google drive

```


```python
from google.colab import files
```


```python
files.download('kp') # finally you can download the file from here to your local computer / else go to Drive and download the same
```

# If you want to make the google drive work as a local drive follow the below


```python
# Install a Drive FUSE wrapper.
# https://github.com/astrada/google-drive-ocamlfuse
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse



# Generate auth tokens for Colab
from google.colab import auth
auth.authenticate_user()


# Generate creds for the Drive FUSE library.
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}


# Create a directory and mount Google Drive using that directory.
!mkdir -p MyDrive
!google-drive-ocamlfuse MyDrive


!ls MyDrive/
```


```python
# Create a directory and mount Google Drive using that directory.
!mkdir -p MyDrive
!google-drive-ocamlfuse MyDrive


!ls MyDrive/
```


```python
!chmod 777 /content/MyDrive/test_dir_2/ # for changing the directory permission Read write 
```


```python
!echo "" > /content/MyDrive/test_dir_2/test # creating a new file at this location a
location = '/content/MyDrive/test_dir_2/test'
```


```python
fo = open(location, "w")
for k in kp_mean.keys():
    fo.write('-1 key:{} mean:{} varience:{} \n'.format(k,kp_mean[k],kp_var[k]))

fo.close()
```
