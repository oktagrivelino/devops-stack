Disarankan untuk menggunakan minimal Ubuntu 18.04 atau Ubuntu 20.04

Pastikan posisi user sebagai ROOT

Lakukan setting masing-masing hostname "sudo hostnamectl set-hostname [Nama Hostname]" dan daftarkan disetiap vm pada "sudo nano /etc/hosts"

Saat melakukan installasi apabila menggunakan script install.sh pastikan melakukan restart OS terlebih dahulu sebelum menjalankan 
command : "kubeadm init......"

Script install.sh dijalankan pada semua vm baik yang akan menjadi master node (control-plane) dan worker node

<-------Bagian Master Node (Control-Plane)-------->

Untuk melakukan setting kubernetes cluster dengan 1 master menggunakan command ini :
"sudo kubeadm init --pod-network-cidr=192.168.0.0/16"

Apabila melakukan setting kubernetes cluster menggunakan multi master menggunakan command ini :
"sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --upload-certs --control-plane-endpoint=[Isi kan nama hostname master utamanya]"

Tunggu beberapa saat hingga selesai dan muncul diline paling akhir :
"kubeadm join ipaddress:6443 --token xxxxxxxxxx --discovery-token-ca-cert-hash sha256:xxxx........"

<-------Bagian Worker Node-------->

Setelah selesai setting pada Master, dan muncul kubeadm join copy command tersebut dan jalankan pada masing-masing worker node
tunggu beberapa saat muncul tulisan "This node has joined the cluster.........."

Kemudian kembali ke master node, dan coba jalan command : "kubectl get nodes"

Apabila saat pertama kali menjalan command "kubectl" terdapat error message seperti ini :
"The connection to the server localhost:8080 was refused – did you specify the correct host or port?"

Lakukan beberapa langkah berikut :

pastikan user sebagai ubuntu atau user biasa
1. mkdir -P $HOME/.kube
2. sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
3. sudo chown $(id -u):$(id -g) $HOME/.kube/config

Kemudian coba jalankan kembali command "kubectl get nodes"

Pada saat awal selesai instalasi status pada control-plane dan worker akan "Not Ready" itu dikarenakan belum terpasangnya CNI pada cluster kubernetes tersebut.

Untuk itu lakukan deploy calico pada cluster agar seluruhnya berstatus READY dan bisa digunakan, lihat file install_calico

Tunggu beberapa saat hingga seluruh pod yang dibuat selesai, untuk memastikannya dapat menjalankan command :
"kubectl get pod -n kube-system" perhatikan pod yang bernama "Calico-kube-controllers-xxxxx dan calico-node-xxxxx" berstatus "RUNNING"
secara otomatis pod "core-dns-xxxx" yang sebelumnya pending akan berubah menjadi "RUNNING"

Selanjutnya bisa menggunakan command "kubectl" sesuai yang dibutuhkan
