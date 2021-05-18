#team5
#Based image
FROM python:2.7-stretch

# maintainers of image
LABEL maintainer = "filrouge.team5@gmail.com"

# update of installed packages
RUN apt-get update -y

# Install libraries2.4.44
RUN apt-get install python-dev=2.7.13-2 python3-dev=3.5.3-1 libsasl2-dev=2.1.27\* libldap2-dev=2.4.44\* libssl-dev -y
RUN pip install flask==1.1.2 flask_httpauth==4.3.0 flask_simpleldap==1.4.0 python-dotenv==0.17.1

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
