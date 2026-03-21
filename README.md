# Doctrack
@app.get("/dashboard")
def dashboard(request: Request, search: str = ""):
    if search:
        cursor.execute(
            """
            SELECT box_id, client, project, description, status
            FROM boxes
            WHERE
                box_id ILIKE %s OR
                client ILIKE %s OR
                project ILIKE %s OR
                description ILIKE %s OR
                status ILIKE %s
            ORDER BY id
            """,
            (
                f"%{search}%",
                f"%{search}%",
                f"%{search}%",
                f"%{search}%",
                f"%{search}%"
            )
        )
    else:
        cursor.execute(
            """
            SELECT box_id, client, project, description, status
            FROM boxes
            ORDER BY id
            """
        )

    rows = cursor.fetchall()

    boxes = []
    for row in rows:
        boxes.append(
            {
                "box_id": row[0],
                "client": row[1],
                "project": row[2],
                "description": row[3],
                "status": row[4]
            }
        )

    total_boxes = len(boxes)
    ready_for_index = sum(1 for box in boxes if box["status"] == "ready_for_index")
    ready_for_qa = sum(1 for box in boxes if box["status"] == "ready_for_qa")
    delivered = sum(1 for box in boxes if box["status"] == "delivered")

    return templates.TemplateResponse(
        "dashboard.html",
        {
            "request": request,
            "boxes": boxes,
            "total_boxes": total_boxes,
            "ready_for_index": ready_for_index,
            "ready_for_qa": ready_for_qa,
            "delivered": delivered,
            "search": search
        }
    )

<form method="get" action="/dashboard" style="margin-bottom: 20px;">
    <input
        type="text"
        name="search"
        placeholder="Search box ID, client, project, status..."
        value="{{ search }}"
        style="padding: 10px; width: 300px; border-radius: 8px; border: 1px solid #ccc;"
    >
    <button
        type="submit"
        style="padding: 10px 16px; border: none; border-radius: 8px; background: #111827; color: white; cursor: pointer;"
    >
        Search
    </button>
</form>
