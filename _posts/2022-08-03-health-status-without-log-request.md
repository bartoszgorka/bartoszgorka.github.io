---
title: "Health status without log request"
image: /assets/images/posts/health-status-without-log-request.png
excerpt: >-
  Sometimes our application collects more logs than we would expect.
  Especially in the case of endpoints responsible for the server status.
  Thanks to a simple plug, you can inform about the system state all the time without logging this request.
date: 2022-08-03 06:00:00 +00:00
last_modified_at: 2022-08-03 06:00:00 +00:00
tags:
  - Elixir
  - code
  - project
  - explanation
  - tips
---

  Tracking user activity is very important in the context of an application.
  I mean the possibility of checking logs regarding actions performed in the system.

  **Sometimes, however, our application collects more logs than expected.**
  A great example is the **redundant logging of endpoints** responsible for the server status, such as `/_health`.

  I want to share with you my way of reducing the number of such entries.
  In my case, each instance is polled for status every five seconds, which gives **12 queries per minute**, 720 per hour, and over **17 000 new logs per day**.
  **For one instance only!**
  Many applications often have several or more instances.

### Health check

  **The easiest way to achieve the goal is to transfer the action from the router directly to the plug:**

  ```elixir
  # lib/my_web_app/router.ex
  # get("/_health", StatusController, :health)

  # lib/my_web_app/plugs/health_check.ex
  defmodule MyWebApp.HealthCheck do
    import Plug.Conn
    @response "All good"

    def init(opts), do: opts

    def call(%Plug.Conn{request_path: "/_health"} = conn, _opts) do
      conn
      |> send_resp(200, @response)
      |> halt()
    end

    def call(conn, _opts), do: conn
  end
  ```

  In my case, I used the `/_health` endpoint; however, you can change it.
  It is **very important to include** `halt(conn)` in your code as this allows you **to stop any further processing** of the request within the existing pipeline.

  It is enough to insert our health check at the beginning of our endpoint configuration.
  It is **important that it is placed before the Logger settings**:

  ```elixir
  # lib/my_web_app/endpoint.ex
  defmodule MyAppWeb.Endpoint do
    use Phoenix.Endpoint, otp_app: :my_app_web

    # Response for /_health path, without logs and tracking
    plug(MyWebApp.HealthCheck)

    ...
  end
  ```

### Summary

  **This way, you can query the status of the application without creating a new entry in the logs.**
  Check for yourself by going to the `<API_URL> _health` page, where you should get information about the success, and at the same time, nothing new should be presented in the console or the log.

  With this small change, you can reduce the number of created logs.
  **It will allow you to focus on important actions instead of browsing additional entries regarding checking the application status.**
