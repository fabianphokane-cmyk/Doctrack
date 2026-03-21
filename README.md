# Doctrack
 * FROM logs
WHERE box_id = 'PHUT-FIN-006'
ORDER BY timestamp ASC;


SELECT * FROM logs ORDER BY timestamp DESC;



@app.get("/boxes/{box_id}/history")
def box_history(request: Request, box_id: str):
    cursor.execute(
        """
        SELECT action, user_name, timestamp
        FROM logs
        WHERE box_id = %s
        ORDER BY timestamp ASC
        """,
        (box_id,)
    )
    rows = cursor.fetchall()

    history = []
    for row in rows:
        history.append(
            {
                "action": row[0],
                "user_name": row[1],
                "timestamp": row[2].strftime("%d %B %Y, %H:%M")
            } 

            
        )

    return templates.TemplateResponse(
        "history.html",
        {
            "request": request,
            "box_id": box_id,
            "history": history
        }
    )

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Box History</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            padding: 30px;
        }

        h1 {
            margin-bottom: 20px;
        }

        .history-box {
            background: white;
            padding: 20px;
            border-radius: 12px;
            max-width: 700px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .event {
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }

        .event:last-child {
            border-bottom: none;
        }

        .time {
            color: #666;
            font-size: 14px;
        }

        a {
            display: inline-block;
            margin-top: 20px;
            text-decoration: none;
            color: #111827;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>History for {{ box_id }}</h1>

    <div class="history-box">
        {% for log in history %}
        <div class="event">
            <strong>{{ log.user_name }}</strong> {{ log.action }}
            <div class="time">{{ log.timestamp }}</div>
        </div>
        {% endfor %}
    </div>

    <a href="/dashboard">← Back to Dashboard</a>
</body>
</html>

<a href="/boxes/{{ box.box_id }}/history">View History</a>


templates = Jinja2Templates(directory="templates")

BASE_DIR = os.path.dirname(os.path.abspath(__file__))
templates = Jinja2Templates(directory=os.path.join(BASE_DIR, "templates"))

  
