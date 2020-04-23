# podman-workshop
Podman workshop: download and modify a container, add to systemd

## Using buildah, podman, and skopeo
```bash
$ skopeo inspect docker://registry.fedoraproject.org/f29/httpd
$ buildah from registry.fedoraproject.org/f29/httpd
$ buildah containers
$ echo 'Welcome to the RHEL8 workshop!' > index.html
$ buildah copy httpd-working-container index.html /var/www/html/index.html
$ buildah config --entrypoint "/usr/bin/run-httpd" httpd-working-container
$ buildah commit httpd-working-container httpd-new-container
$ buildah containers
$ buildah images
$ podman images
$ podman run -d -p 8080:8080 localhost/httpd-new-container
$ podman ps
$ podman top -l
$ podman unshare
# podman ps
# podman mount <container id>
# cd /home/*user*/.local/share/containers/storage/overlay/*long-image-id*/merged
# cat var/www/html/index.html
# exit
$ curl -s http://localhost:8080
Welcome to the RHEL8 workshop!
$ podman stop -a
$ podman ps -l
```
### Other resources:
https://www.redhat.com/sysadmin/behind-scenes-podman

## Manage edge nodes with SystemD
```bash
$ podman generate systemd --name <container name>
$ mkdir -p ~/.config/systemd/user
$ cd
$ git clone https://github.com/dthurston/systemd-unit-example.git
$ cd ~/.config/systemd/user
$ cp ~/systemd-unit-example/container-httpd.service .
$ vi container-httpd.service
$ systemctl --user enable container-httpd.service --now
$ systemctl --user status container-httpd.service
$ systemctl --user daemon-reload # when changes are made to the unit file and it is currently running
```
### Other resources:
https://github.com/rlucente-se-jboss/systemd-podman-edge
[Running containers with Podman and shareable systemd services](https://www.redhat.com/sysadmin/podman-shareable-systemd-services)
[Dockerless: Build and Run Containers with Podman and Systemd](https://www.youtube.com/watch?v=RfL_CjXfQds)
https://www.openshift.com/try
