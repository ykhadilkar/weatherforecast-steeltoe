SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='docker.io/ykhadilkar/weatherforecast-steeltoe') # update registry
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')
NAME = "weatherforecast-steeltoe"

k8s_custom_deploy(
  NAME,
  apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
            " --local-path " + LOCAL_PATH +
            " --source-image " + SOURCE_IMAGE +
            " --build-env BP_DEBUG_ENABLED=true" +
            " --namespace " + NAMESPACE +
            " --yes " +
            OUTPUT_TO_NULL_COMMAND +
            " && kubectl get workload " + NAME + " --namespace " + NAMESPACE + " -o yaml",
  delete_cmd="tanzu apps workload delete " + NAME + " --namespace " + NAMESPACE + " --yes",
  deps=['./bin'],
  container_selector='workload',
  live_update=[
    sync('./bin', '/workspace')
  ]
)

k8s_resource(NAME, port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'weatherforecast-steeltoe', 'app.kubernetes.io/component': 'run'}])

allow_k8s_contexts('<context>') # update the context
