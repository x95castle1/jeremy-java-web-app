SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor-repo.vmware.com/tapsme/supply-chain/another-inner-loop')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'another-inner-loop',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null " +
              "&& kubectl get workload another-inner-loop --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    image_selector='harbor-repo.vmware.com/tapsme/supply-chain/another-inner-loop-' + NAMESPACE,
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('another-inner-loop', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'another-inner-loop'}])

allow_k8s_contexts('unicorn-admin@unicorn')