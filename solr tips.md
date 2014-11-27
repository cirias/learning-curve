1. If `defType=<dismax|edismax>`, `q=` will not work. Use `q.alt=*:*`
2. Reload conf file without restart: http://<solr server host:8983>/solr/admin/cores?action=RELOAD&core=<core name>
3. Use or with fq: `fq=fieldname:(value1 or value2)`
