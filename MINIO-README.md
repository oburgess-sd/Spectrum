# Installation via yaml

Followed link from https://min.io/docs/minio/kubernetes/upstream/index.html#procedure4


# Installation via Krew
## Installation of Krew

1) Install git
2) run git config --global --add safe.directory C:/Users/YOURUSERNAME/.krew/index/default
3) Follow instructions here: https://krew.sigs.k8s.io/docs/user-guide/setup/install/

## Installation of MINIO

1) Follow instructions here: https://min.io/docs/minio/kubernetes/upstream/operations/installation.html
2) I had issues with:


    kubectl krew install minio


    Updated the local copy of plugin index.
    Installing plugin: minio
    W0227 20:37:30.015318   15960 install.go:164] failed to install plugin "minio": install failed: failed to link installed plugin: failed to create a symlink from "C:\\Users\\oburgess\\.krew\\store\\minio\\v4.5.8\\kubectl-minio.exe" to "C:\\Users\\oburgess\\.krew\\bin\\kubectl-minio.exe": symlink C:\Users\oburgess\.krew\store\minio\v4.5.8\kubectl-minio.exe C:\Users\oburgess\.krew\bin\kubectl-minio.exe: A required privilege is not held by the client.
    failed to install some plugins: [minio]: install failed: failed to link installed plugin: failed to create a symlink from "C:\\Users\\oburgess\\.krew\\store\\minio\\v4.5.8\\kubectl-minio.exe" to "C:\\Users\\oburgess\\.krew\\bin\\kubectl-minio.exe": symlink C:\Users\oburgess\.krew\store\minio\v4.5.8\kubectl-minio.exe C:\Users\oburgess\.krew\bin\kubectl-minio.exe: A required privilege is not held by the client.
    Error: exit status 1

Fix was to copy the .exe over manually to the bin folder