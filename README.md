<th>Action</th> 


<td>
    <form action="/scanner/mark_ready/{{ box.box_id }}" method="post">hh
        <button type="submit">Send to Indexing</button>
    </form>
</td>


@app.post("/scanner/mark_ready/{box_id}")
def mark_ready(box_id: str):
    cursor.execute(
        "UPDATE boxes SET status = %s WHERE box_id = %s",
        ("ready_for_index", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/scanner", status_code=303)


from fastapi.responses import RedirectResponse


@app.post("/scanner/mark_ready/{box_id}")
def mark_ready(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("ready_for_index", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/scanner", status_code=303)




@app.post("/indexing/send_to_qa/{box_id}")
def send_to_qa(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("ready_for_qa", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/indexing", status_code=303)


    <td>
    <form action="/indexing/send_to_qa/{{ box.box_id }}" method="post">
        <button type="submit">Send to QA</button>
    </form>
</td>











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






<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QA Queue</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            padding: 30px;
        }

        h1 {
            margin-bottom: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
        }

        th, td {
            padding: 14px;
            border-bottom: 1px solid #eee;
            text-align: left;
        }

        th {
            background: #111827;
            color: white;
        }

        button {
            padding: 8px 12px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            margin-right: 8px;
        }
    </style>
</head>
<body>
    <h1>QA Queue</h1>

    <table>
        <thead>
            <tr>
                <th>Box ID</th>
                <th>Client</th>
                <th>Project</th>
                <th>Description</th>
                <th>Status</th>
                <th>Action</th>
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
                <td>
                    <form action="/qa/approve/{{ box.box_id }}" method="post" style="display:inline;">
                        <button type="submit">Approve</button>
                    </form>

                    <form action="/qa/reject/{{ box.box_id }}" method="post" style="display:inline;">
                        <button type="submit">Reject</button>
                    </form>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</body>
</html>


@app.post("/qa/approve/{box_id}")
def qa_approve(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("qa_passed", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/qa", status_code=303)


@app.post("/qa/reject/{box_id}")
def qa_reject(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("ready_for_index", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/qa", status_code=303)








    
