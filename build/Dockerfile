FROM quay.io/operator-framework/ansible-operator:v0.11.0

LABEL org.kubevirt.hco.csv-generator.v1="/usr/bin/csv-generator"

COPY roles/ ${HOME}/roles/
COPY playbooks/ ${HOME}/
COPY watches.yaml ${HOME}/watches.yaml
COPY _defaults.yml ${HOME}/_defaults.yml

COPY deploy/crds ${HOME}/deploy/crds
COPY manifests/generated ${HOME}/manifests/generated
COPY build/csv-generator.sh /usr/bin/csv-generator
#original module k8s_status contains bugs, which prevents ssp operator to correctly set conditions
#https://github.com/operator-framework/operator-sdk/pull/1820
#https://github.com/operator-framework/operator-sdk/pull/1845
COPY build/k8s_status.py /usr/share/ansible/openshift

COPY build/preprocess_template.py ${HOME}/
COPY patch.yaml ${HOME}/
RUN python3 ${HOME}/preprocess_template.py ${HOME}/patch.yaml ${HOME}/roles/KubevirtCommonTemplatesBundle/files/
