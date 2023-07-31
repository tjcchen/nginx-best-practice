## Nginx Running Principle and Architecture


![architecture](https://github.com/tjcchen/nginx-best-practice/assets/6133656/01894dc5-ee28-4ff0-8628-7d683a511a44)


NGINX follows the master-slave architecture by supporting event-driven, asynchronous and non-blocking model.



## NGINX has sliced into three different parts

- Master
- Workers
- Cache



**MASTER:**

As mentioned NGINX follows the master-slave architecture. It’ll allocate the jobs for the workers as per the request from the client. Once the job allocated to the workers, the master will look for the next request from the client that’s it won’t wait for the response from the workers. Once the response comes from workers, master will send the response to the client.



**WORKERS:**

Workers are the slaves in the NGINX architecture, will heed to the Master. Each worker can handle more than 1000 request at a time in a single-threaded manner. Once the process gets done, the response will be sent to the master. The single-threaded will saves the RAM and ROM size by working on the same memory space instead of different memory spaces. The multi-threaded will work on different memory spaces.



**Cache:**

Nginx cache is used to render the page very fast by getting from cache memory instead of getting from the server. The pages are getting stored in cache memory on the first request for the page.
