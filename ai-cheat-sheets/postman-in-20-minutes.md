# Postman in 20 minutes

Never used Postman, or only a bit? This is all you need for today. Do it with your partner: one of you clicks, the other one reads along. The web version (https://web.postman.co, free account) works just as well as the desktop app, so you don't have to install anything.

Postman is a tool to send HTTP requests to an API and look at what comes back. That's all. You'll use it to talk to RetroFlow.

## 1. Make a collection with a variable

A **collection** is a folder for your requests. A **variable** is a name for a value you use everywhere, so you don't type the URL fifty times.

1. Create a new collection and call it `RetroFlow`.
2. Open the collection, go to the **Variables** tab and add a variable `baseUrl`. Put your group's URL in the **Current value** column, without a slash at the end. Your coach gives you the URL. Save.
3. From now on, every request starts with `{{baseUrl}}`. For example: `{{baseUrl}}/api/teams`.

## 2. Send a GET request

1. In the collection, add a request. Choose **GET**.
2. URL: `{{baseUrl}}/api/teams/1`
3. Press **Send**.

Look at the bottom half of the screen. There are three things you can read:

| What | Where | Why |
|---|---|---|
| **Status code** | Top right of the response, for example `200 OK` or `404 Not Found` | Tells you if it worked. 2xx is success, 4xx means your request was wrong, 5xx means the application broke |
| **Body** | The big area, tab **Body**, set to **Pretty** and **JSON** | What the application sent back. This is where the details are |
| **Headers** | Tab **Headers** | Rarely needed today |

A `404` with a JSON message for team 1 is fine. It means the app is up and team 1 doesn't exist.

## 3. Send a POST request with a body

A POST sends data. For RetroFlow, you create a team like this:

1. Add a new request. Choose **POST**. URL: `{{baseUrl}}/api/teams`
2. Open the **Body** tab, choose **raw**, and in the dropdown on the right choose **JSON**.
3. Paste:

```json
{
  "name": "Team CM-Phoenix",
  "members": ["Alice", "Bob"]
}
```

4. Use **your own initials** in the name instead of CM: your whole group shares this app. Press **Send**.

Postman sets the `Content-Type: application/json` header for you when you choose JSON. If you ever get a `415`, or a `400` with a strange message, check the **Headers** tab for it.

In the response you get the new team, including an `id`. That's the number you use in other requests, for example `GET {{baseUrl}}/api/teams/<that id>`. Do that now: if you get your team back, you did it right.

## 4. Use the id in the next request

Look in `retroflow-api-reference.md`. To create a retrospective for your team, you POST to `{{baseUrl}}/api/teams/<teamId>/retrospectives` with `{"title": "My first retro"}`. Do it, and read the response.

Save every request in your collection with a clear name (`Create team`, `Close retrospective`). In Block 1 your test cases become requests you can send again, and your partner can reuse them.

## 5. Write down what came back

You'll write results in `test-cases.md`, `session-notes.md` and `bugs.md` all day. Always write down both:

* the **status code**, exactly: `201`, not "it worked"
* the part of the **body** that matters, copied: `"status":"OPEN"`, or the error message

Tip: click in the response body, select all and copy. Paste it in your notes, then remove what doesn't matter. A pasted response is evidence. "It gave an error" is not.

## When something doesn't work

Ask your AI chat, and give it the **request** (method, URL, body) and the **response** (status and body), copy-pasted. For example:

```
I sent this request to a REST API: [method, URL, body]. I got this response: [status and body].
Explain what the response means and what I probably did wrong. Don't guess about the API
beyond what you see here.
```

And the reverse. If you don't know how to build a request:

```
I use Postman. Give me the exact method, URL and JSON body to [create a retrospective for team 5]
based on this API reference: [paste the relevant lines].
```

Then **check the answer against the API reference**. The AI may invent a path.

## The five mistakes everybody makes

- Forgetting `{{baseUrl}}`, and typing half a URL
- Forgetting to choose **raw** and **JSON** in the Body tab
- Putting the URL in the wrong column of the variable, or leaving it empty (the URL then shows in red)
- Using an id that belongs to the other pair's data, or one that doesn't exist yet
- Reading only the status code and not the body

Still stuck after 10 minutes? Ask the other pair of your group or your coach. Show them the request and the response.
