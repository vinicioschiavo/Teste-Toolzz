# Teste-Toolzz
Teste para vaga de devops para a empresa Toolzz

# Comandos para instalar o Helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

# Instalar o kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl


## Comando para instalar ou atualizar o flowise via helm
helm repo add cowboysysop https://cowboysysop.github.io/charts/
helm upgrade --install flowise cowboysysop/flowise --namespace flowise

aws eks update-kubeconfig --name vi-cluster
kubectl create namespace flowise
kubectl config set-context arn:aws:eks:us-west-2:399679827371:cluster/vi-cluster --namespace "default ou flowise" para mudar