# model_train

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
!mkdir -p drive
!google-drive-ocamlfuse drive

# Create a folder to use the competition
!mkdir -p /content/drive/tut_competition/

!ls /content/drive/tut_competition/

# Run the colab_test.py
!python3 ~/my_drive/tut_kaggle/colab_test.py

import tensorflow as tf
tf.test.gpu_device_name()

!pip install -q keras

!pip install tensorflow-gpu

# Run to classify and to create the submission file
!python3 /content/drive/tut_competition/capsulenet_colab.py


#folder tree structure

/content/drive/tut_competition/Thesis-2018/data/test.py
