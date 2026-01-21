FROM python:3.13.11-slim

RUN pip install pandas pyarrow

WORKDIR /app

COPY pipeline.py /app/my_pipeline.py

ENTRYPOINT ["python", "my_pipeline.py"]
