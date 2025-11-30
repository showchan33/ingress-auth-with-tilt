load('ext://dotenv', 'dotenv')
dotenv()

registry_host = os.getenv('REGISTRY_HOST', 'localhost:5000')
default_registry(registry_host)

k8s_context = os.getenv('K8S_CONTEXT', None)
if k8s_context: 
  allow_k8s_contexts(k8s_context)

docker_build(
  'ts-form-auth-webapp',
  './ts-form-auth-webapp',
  dockerfile='./ts-form-auth-webapp/Dockerfile'
)

k8s_namespace = os.getenv('K8S_NAMESPACE', 'default')
k8s_yaml(helm('./helm-chart', namespace=k8s_namespace))
