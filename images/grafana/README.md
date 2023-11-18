
## savatin/grafana

Run this [Grafana](http://grafana.org/) 2.x image with:

    $ mkdir /var/lib/grafana
    $ docker run -d --name grafana-0 \
       -v /var/lib/grafana:/opt/grafana/data \
       -p 3000:3000 savatin/grafana

Web interface:

    http://localhost:3000

Default admin credentials:

    admin/admin

---

![PACKAGES](PACKAGES.md)
