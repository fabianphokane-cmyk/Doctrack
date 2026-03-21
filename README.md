# Doctrack
CREATE TABLE logs (
    id SERIAL PRIMARY KEY,
    box_id VARCHAR(50) NOT NULL,
    action VARCHAR(100) NOT NULL,
    user_name VARCHAR(100) NOT NULL,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

def add_log(box_id: str, action: str, user_name: str):
    cursor.execute(
        """
        INSERT INTO logs (box_id, action, user_name)
        VALUES (%s, %s, %s)
        """,
        (box_id, action, user_name)
    )
    conn.commit()

    
add_log(box_id, "scanned", "Ian")

add_log(box_id, "indexed", "Sarah")

add_log(box_id, "approved QA", "John")

add_log(box_id, "marked delivered", "Admin")

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
        history.append(
            {
                "action": row[0],
                "user_name": row[1],
                "timestamp": row[2].strftime("%d %B %Y, %H:%M")
            }
        )

    return {
        "box_id": box_id,
        "history": history
    }
