# Laby7_Wojciech_Wojciechewicz

## 1. Utworzenie przestrzeni nazw

kubectl create ns remote

## 2. Uruchomienie Poda remoteweb (nginx) w namespace remote

kubectl run remoteweb -n remote --image=nginx

## 3. Utworzenie Service typu NodePort z portem 31999

kubectl expose pod remoteweb -n remote --type=NodePort --port=80 --name=remoteweb
kubectl patch svc remoteweb -n remote -p '{"spec": {"ports": [{"port":80,"protocol":"TCP","targetPort":80,"nodePort":31999}]}}'

## 4. Uruchomienie Poda testpod w namespace default

kubectl run testpod --image=busybox --command -- sleep infinity

## 5. Test połączenia z testpod (wewnątrz klastra)

kubectl exec -it testpod -- sh
wget --spider --timeout=1 http://remoteweb.remote.svc.cluster.local
exit

## 6. Test połączenia z zewnątrz przez NodePort 31999
Pobranie adresu IP minikube

minikube ip

curl http://$(minikube ip):31999

## 7. Wnioski
1. Usługa remoteweb jest dostępna z wewnątrz klastra poprzez DNS Kubernetes.
2. Poprawnie wystawiona jako NodePort jest również dostępna z zewnątrz na porcie 31999.
    Usługa remoteweb jest dostępna z wewnątrz klastra poprzez DNS Kubernetes.

    Poprawnie wystawiona jako NodePort jest również dostępna z zewnątrz na porcie 31999.
