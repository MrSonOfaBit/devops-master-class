FROM python:alpine3.10
WORKDIR /app
COPY requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . /app
ENTRYPOINT ["python", "launch.py"]
CMD ["echo", "Hello World"]
#COPY requirements.txt /app/requirements.txt
#ENTRYPOINT ["python", "./launch.py"]
