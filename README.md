[![Deploy to Tutum](https://s.tutum.co/deploy-to-tutum.svg)](https://dashboard.tutum.co/stack/deploy/)

This Docker image helps you to run Spark (on Docker) with the following installed:

1. [[https://spark.apache.org/][pySpark]] (Spark 1.1.0) on Hadoop 2.5.1
2. python 2.6.6
3. numpy 1.9.0
4. scipy 0.14.0
5. scikit-learn 0.15.2

# Setting up (one-time only)

## On MacOSX:
### 1. Install [homebrew](http://brew.sh)
### 2. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
### 3. Install boot2docker

Run the following commands in your MacOS terminal.

```
brew install boot2docker
boot2docker init
boot2docker up
```
# The following URL is output as a result of the above "boot2docker up" command.
```
export DOCKER_HOST=tcp://192.168.59.103:2375
```


### 4. Install docker:

Run the following commands in your MacOS terminal.

```
brew install docker
docker version
```


## On other OSes

Make sure that docker is installed and is runnable from the command-line.

# Starting pyspark


## 1. Pull the docker image

Run the following commands in your MacOS terminal (make sure that
=$DOCKER_HOST= is set correctly)

```
docker build -t pyspark github.com/mpetyx/pyspark-docker
```


## 2. Start the container

Run the following command to start the container and get a bash prompt

```
docker run -i -t -h sandbox pyspark /etc/bootstrap.sh -bash
```

## 3. Start pyspark

```
/usr/local/spark/bin/pyspark
```

This should place you in a python prompt (=>>>=)
## 4. Verify installation

To verify pyspark, run the following example Spark program:
```
data = [1, 2, 3, 4, 5]
sc.parallelize(data).count()
```

This should print a bunch of debugging output, and on the last line,
it should print the output, "5"

To verify scikit-learn, run the following example program:

```
from sklearn import svm, datasets
clf = svm.SVC(gamma=0.001, C=100.)
digits = datasets.load_digits()
clf.fit(digits.data[:-1], digits.target[:-1])
```

You should see output like:
```
SVC(C=100.0, cache_size=200, class_weight=None, coef0=0.0, degree=3,
  gamma=0.001, kernel='rbf', max_iter=-1, probability=False,
  random_state=None, shrinking=True, tol=0.001, verbose=False)
```

# (OPTIONAL) Building the docker image yourself

You can build this docker image, by running the following command in
the same directory as this =README= file. The command will be slow (a
few minutes) the first time, since numpy and scikitlearn need to be
compiled from source, but the result is then cached. This step should
only be necessary if you modify =Dockerfile=

```
$ docker build -t pyspark-docker .
```

# Troubleshooting
If you are unable to access HDFS from pyspark, try running pyspark with the =--master yarn= flag.
