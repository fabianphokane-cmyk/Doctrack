# Doctrack
@app.get("/boxes/{box_id}/history")
def box_history(box_id: str):
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

    return {
        "box_id": box_id,
        "history": history
    }

        
