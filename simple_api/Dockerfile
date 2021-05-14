#team5
#Based image
FROM python:2.7-stretch

# maintainers of image
LABEL maintainer = "filrouge.team5@gmail.com"

# update of installed packages
RUN apt-get update -y

# Install libraries
RUN apt-get install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip install flask flask_httpauth flask_simpleldap python-dotenv

# pip's update
RUN apt-get update -y


# copy source code
ADD student_age.py/  /

VOLUME [ "/data" ]
# create data folder

# expose port
EXPOSE 5000

# run code
CMD [ "python", "./student_age.py" ]
