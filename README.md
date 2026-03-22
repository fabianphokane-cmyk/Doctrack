# Doctrack
@app.get("/boxes/{box_id}/history/view")
def box_history_view(request: Request, box_id: str):
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
        history.append({
            "action": row[0],
            "user_name": row[1],
            "timestamp": str(row[2])
        })

    return templates.TemplateResponse(
        "history.html",
        {
            "request": request,
            "box_id": box_id,
            "history": history
        }
    )

<a href="/boxes/{{ box.box_id }}/history/view">View History</a>

    
