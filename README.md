@app.get("/scanner")
def scanner_page(request: Request):
    cursor.execute(
        """
        SELECT box_id, client, project, description, status
        FROM boxes
        WHERE status = %s
        ORDER BY id
        """,
        ("received",)
    )
    rows = cursor.fetchall()

    boxes = []
    for row in rows:
        boxes.append({
            "box_id": row[0],
            "client": row[1],
            "project": row[2],
            "description": row[3],
            "status": row[4]
        })

    return templates.TemplateResponse(
        "scanner.html",
        {
            "request": request,
            "boxes": boxes
        }
    )


@app.get("/indexing")
def indexing_page(request: Request):
    cursor.execute(
        """
        SELECT box_id, client, project, description, status
        FROM boxes
        WHERE status = %s
        ORDER BY id
        """,
        ("ready_for_index",)
    )
    rows = cursor.fetchall()

    boxes = []
    for row in rows:
        boxes.append({
            "box_id": row[0],
            "client": row[1],
            "project": row[2],
            "description": row[3],
            "status": row[4]
        })

    return templates.TemplateResponse(
        "indexing.html",
        {
            "request": request,
            "boxes": boxes
        }
    )


@app.get("/qa")
def qa_page(request: Request):
    cursor.execute(
        """
        SELECT box_id, client, project, description, status
        FROM boxes
        WHERE status = %s
        ORDER BY id
        """,
        ("ready_for_qa",)
    )
    rows = cursor.fetchall()

    boxes = []
    for row in rows:
        boxes.append({
            "box_id": row[0],
            "client": row[1],
            "project": row[2],
            "description": row[3],
            "status": row[4]
        })

    return templates.TemplateResponse(
        "qa.html",
        {
            "request": request,
            "boxes": boxes
        }
    )


@app.get("/delivery")
def delivery_page(request: Request):
    cursor.execute(
        """
        SELECT box_id, client, project, description, status
        FROM boxes
        WHERE status = %s
        ORDER BY id
        """,
        ("qa_passed",)
    )
    rows = cursor.fetchall()

    boxes = []
    for row in rows:
        boxes.append({
            "box_id": row[0],
            "client": row[1],
            "project": row[2],
            "description": row[3],
            "status": row[4]
        })

    return templates.TemplateResponse(
        "delivery.html",
        {
            "request": request,
            "boxes": boxes
        }
    )

    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scanner Queue</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f6f8; padding: 30px; }
        h1 { margin-bottom: 20px; }
        table { width: 100%; border-collapse: collapse; background: white; }
        th, td { padding: 14px; border-bottom: 1px solid #eee; text-align: left; }
        th { background: #111827; color: white; }
    </style>
</head>
<body>
    <h1>Scanner Queue</h1>
    <table>
        <thead>
            <tr>
                <th>Box ID</th>
                <th>Client</th>
                <th>Project</th>
                <th>Description</th>
                <th>Status</th>
            </tr>
        </thead>
        <tbody>
            {% for box in boxes %}
            <tr>
                <td>{{ box.box_id }}</td>
                <td>{{ box.client }}</td>
                <td>{{ box.project }}</td>
                <td>{{ box.description }}</td>
                <td>{{ box.status }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</body>
</html>




<title>Indexing Queue</title>
<h1>Indexing Queue</h1>

<title>QA Queue</title>
<h1>QA Queue</h1>


<title>Delivery Queue</title>
<h1>Delivery Queue</h1>





    
