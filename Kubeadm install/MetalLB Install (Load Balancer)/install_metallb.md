Jalankan perintah berikut ini :

kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.15.2/config/manifests/metallb-native.yaml

Kemudian buat 1 file lagi dengan tujuan memasukkan range ip address yang nantinya akan digunakan, karena secara default untuk installasi metallb diatas hanya membuat listener dan speaker saja.
Sehingga butuh dilakukan pembuatan ip pool range yang dapat digunakan.

Buat file yaml dengan nama "ipaddresspool.yaml" yang berisikan yaml pada seperti pada ipaddresspool.yaml didalam folder ini.

Kedua buat file yaml dengan nama "l2advertisement.yaml" yang berisikan yaml pada seperti pada l2advertisement.yaml didalam folder ini.

Setelah itu apply kedua file tersebut 

kubectl apply -f ipaddresspool.yaml
kubectl apply -f l2advertisement.yaml

Setelah berhasil, selanjutnya dapat kita coba untuk membuat sebuah deployment dan mengekspose service dari deployment tersebut menggunakan service type LoadBalancer

kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0


kubectl expose deployment hello-server --type LoadBalancer --port 80 --target-port 8080

Kemudian cek menggunakan command kubectl get service <nama_deployment> [Name_Space jika menggunakan namespace tertentu]

IP External yang muncul pada kolam EXTERNAL-IP dapat digunakan untuk mengakses aplikasi melalui browser menandakan installasi Metallb sudah selesai.
