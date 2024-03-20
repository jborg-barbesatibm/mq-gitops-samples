ARG mqSrcImage="icr.io/ibm-messaging/mq"

ARG mqVersion="9.3.3.1-r1"

ARG targetRegistry="image-registry.openshift-image-registry.svc:5000/mq-demo"

FROM ${targetRegistry}/mq-metrics:${mqVersion} as mq-metrics

FROM ${mqSrcImage}:${mqVersion}


USER 0

RUN cat /etc/os-release && echo "Architecture is: " && arch

# Install new metrics support:
RUN mkdir -p /opt/mqm/metrics

COPY --from=mq-metrics /go/bin/mq_prometheus /usr/local/bin/mqgo/mq_prometheus
RUN chmod 770 /usr/local/bin/mqgo/mq_prometheus

RUN mkdir -p /tmp/source
COPY * /tmp/source
RUN chmod 666 /tmp/source
RUN ls -l /tmp/source
# RUN ls --recursive /tmp/source

# Use my own instance of the script
# COPY --from=mq-metrics /go/src/github.com/ibm-messaging/mq-metric-samples/mq_prometheus.sh /opt/mqm/metrics
COPY mq_prometheus.sh /opt/mqm/metrics
    
RUN chmod 770 /opt/mqm/metrics/*


USER 1001

