# Movie Script Analysis using Hadoop MapReduce

## Project Overview
This project implements a Hadoop MapReduce job to analyze movie script dialogues. The program processes a dataset where each line follows the format:
```
Character: Dialogue
```
It extracts insights such as the most frequently spoken words, total dialogue length per character, and unique words used by each character while leveraging Hadoop Counters for key statistics.

## Approach and Implementation
The project consists of the following components:

### Mapper
- Extracts the character name and dialogue.
- Tokenizes dialogue into words.
- Emits word counts, dialogue length, and unique words for each character.
- Increments Hadoop Counters for total lines, words, characters, and unique words.

### Reducer
- Aggregates word counts per character.
- Computes the total dialogue length per character.
- Collects unique words spoken by each character.
- Outputs the final structured results.

## Execution Steps
### 1. Start Docker Environment

docker compose up -d
```
### 2. Build the Project

mvn install

### 3. Move JAR File and Input Data to Docker Container
mv target/*.jar input/
docker cp input/hands-on2-movie-script-analysis-1.0-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
docker cp input/movie_dialogues.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
### 4. Run Hadoop MapReduce Job

docker exec -it resourcemanager /bin/bash
hadoop fs -mkdir -p /input/dataset
hadoop fs -put ./movie_dialogues.txt /input/dataset
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/hands-on2-movie-script-analysis-1.0-SNAPSHOT.jar com.example.controller.Controller /input/dataset/movie_dialogues.txt /output
```

###5.Connect to Docker 

docker exec -it resourcemanager /bin/bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
hadoop fs -mkdir -p /input/dataset
hadoop fs -put ./input.txt /input/dataset

### 6. Retrieve Output

hadoop fs -cat /output/*
hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
exit
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ output/
```
### 7. Commit Results to GitHub

git add .
git commit -m "MY HANDS ON MAPREDUCE MOVIE_DIALOGUE"
git push origin main

