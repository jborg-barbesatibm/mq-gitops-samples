ARG mqSrcImage="icr.io/ibm-messaging/mq"

ARG mqVersion="9.3.3.1-r1"

ARG targetRegistry="image-registry.openshift-image-registry.svc:5000/mq-demo"

FROM ${targetRegistry}/mq-metrics:${mqVersion} as mq-metrics

FROM ${mqSrcImage}:${mqVersion}

RUN apt-get update && apt-get upgrade -y
RUN apt-get install libssl-dev

# RUN apt-get install -y -q build-essential curl
# RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
# ENV PATH="/root/.cargo/bin:${PATH}"

USER 0

# Add support for go:
RUN rpm -qa
RUN microdnf --disableplugin=subscription-manager install openssl-devel
RUN microdnf --disableplugin=subscription-manager clean all


# Install new metrics support:
RUN mkdir -p /opt/mqm/metrics

COPY --from=mq-metrics /go/bin/mq_prometheus /usr/local/bin/mqgo/mq_prometheus

RUN chmod 770 /usr/local/bin/mqgo/mq_prometheus

COPY --from=mq-metrics /go/src/github.com/ibm-messaging/mq-metric-samples/mq_prometheus.sh /opt/mqm/metrics

RUN chmod 770 /opt/mqm/metrics/*

USER 1001
