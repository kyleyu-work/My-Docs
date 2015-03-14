# Junit Installation


1. Download the latest version of [JUnit](http://sourceforge.net/projects/junit/files/), referred to below as `junit-4.10.jar`
2. Unzip the junit.zip distribution file to a directory referred to as `$JUNIT_HOME`.
```bash
jar -xvf junit-4.10.jar  (put the extracted files in folder JUNIT)
export JUNIT_HOME=~/Developer/Framework/JUNIT
```

3. Add junit to classpath
```bash
export CLASSPATH=$JUNIT_HOME:$CLASSPATH   (optional)
export CLASSPATH=$JUNIT_HOME/junit-4.10.jar:$CLASSPATH
```

