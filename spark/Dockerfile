FROM infotechsoft/hadoop:latest

ARG SPARK_VERSION=2.4.0
ENV SPARK_VERSION ${SPARK_VERSION}
ENV SPARK_HOME=/opt/spark-$SPARK_VERSION

RUN set -x \
    && . /functions.sh \
    && apache_install spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-without-hadoop.tgz spark/KEYS \
    && mv /opt/spark-$SPARK_VERSION-* $SPARK_HOME

WORKDIR $SPARK_HOME
ENV PATH $SPARK_HOME/bin:$PATH

ADD spark-entrypoint.sh /
ADD spark-historyserver.sh /
ADD spark-master.sh /
ADD spark-slave.sh /

ADD gcs-connector-latest-hadoop2.jar spark-bigquery-with-dependencies_2.11-0.14.0-beta.jar guava-23.0.jar /opt/spark-2.4.0/jars/  
ADD spark-sa.json /opt/
ADD core-site.xml /opt/hadoop-2.9.1/etc/hadoop/
ADD spark-env.sh /opt/spark-2.4.0/conf/
RUN chmod a+x \
    /spark-entrypoint.sh \
    /spark-historyserver.sh \
    /spark-master.sh \
    /spark-slave.sh

ENV GS_PROJECT_ID=sincere-charmer-268311    
RUN echo "export SPARK_DIST_CLASSPATH=$(hadoop classpath)" >> /opt/spark-$SPARK_VERSION/conf/spark-env.sh
RUN export $SPARK_DIST_CLASSPATH
RUN echo "export LD_LIBRARY_PATH=$HADOOP_PREFIX/lib/native/" >> /opt/spark-$SPARK_VERSION/conf/spark-env.sh 
ENTRYPOINT ["/spark-entrypoint.sh"]
