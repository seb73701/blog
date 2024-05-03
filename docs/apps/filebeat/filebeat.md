# FILEBEAT
---

## <i class="fa-solid fa-hashtag"></i> Information

![Logo](../../_media/apps/filebeat/filebeat_logo.png ':size=x250 :no-zoom')


> <i class="fa-solid fa-quote-left"></i> **Filebeat** is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, **Filebeat** monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Here’s how **Filebeat** works: When you start **Filebeat**, it starts one or more inputs that look in the locations you’ve specified for log data. For each log that **Filebeat** locates, **Filebeat** starts a harvester. Each harvester reads a single log for new content and sends the new log data to libbeat, which aggregates the events and sends the aggregated data to the output that you’ve configured for **Filebeat**. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://www.elastic.co/fr/beats/filebeat
- <i class="fa-solid fa-book"></i> **Documentation** : https://www.elastic.co/guide/en/beats/filebeat/current/index.html
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://www.elastic.co/guide/en/beats/libbeat/7.14/release-notes-7.14.0.html
- <i class="fa-brands fa-github"></i> **Github** : https://github.com/elastic/beats/tree/master/filebeat
- <i class="fab fa-blogger-b"></i> **Blog** : https://www.elastic.co/blog
- <i class="fas fa-comments"></i> **Forum** : https://discuss.elastic.co/c/beats



## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [A Filebeat Tutorial: Getting Started](https://logz.io/blog/filebeat-tutorial/)
- [Collecting and Shipping Kubernetes logs at scale with FileBeat Autodiscover](https://logz.io/learn/webinar-collecting-and-shipping-kubernetes-logs-at-scale-with-filebeat-autodiscover/)
- [How to deal with many small log files using Filebeat](https://blog.sleeplessbeastie.eu/2020/12/02/how-to-deal-with-many-small-log-files-using-filebeat/)
- [Shipping Multiline Logs with Filebeat](https://logz.io/blog/shipping-multiline-logs-with-filebeat/)
- [Structured logging with Filebeat](https://www.elastic.co/blog/structured-logging-filebeat)
- [Structured logging with Filebeat](https://www.elastic.co/fr/blog/structured-logging-filebeat)
- [What is Autodiscover for Filebeat? And why do we need it?](https://logz.io/blog/what-is-autodiscover-filebeat/)


</details>
