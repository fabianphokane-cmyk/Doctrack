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



new.....new.....new 


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
    <title>Delivery Queue</title>
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
        }
    </style>
</head>
<body>
    <h1>Delivery Queue</h1>

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
                    <form action="/delivery/mark_delivered/{{ box.box_id }}" method="post" style="display:inline;">
                        <button type="submit">Mark Delivered</button>
                    </form>
                </td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</body>
</html>

@app.post("/delivery/mark_delivered/{box_id}")
def mark_delivered(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("delivered", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/delivery", status_code=303)



new new new v2 VeV2 

@app.get("/")
def home(request: Request):
    return templates.TemplateResponse(
        "home.html",
        {"request": request}
    )


    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DocTrack Home</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            margin: 0;
            padding: 40px;
        }

        h1 {
            margin-bottom: 10px;
            color: #111827;
        }

        p {
            color: #555;
            margin-bottom: 30px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            max-width: 1000px;
        }

        .card {
            background: white;
            padding: 24px;
            border-radius: 14px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            text-decoration: none;
            color: inherit;
            transition: transform 0.15s ease, box-shadow 0.15s ease;
        }

        .card:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 16px rgba(0,0,0,0.12);
        }

        .card h2 {
            margin: 0 0 8px;
            font-size: 22px;
            color: #111827;
        }

        .card p {
            margin: 0;
            color: #666;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <h1>DocTrack</h1>
    <p>Select a section to continue.</p>

    <div class="grid">
        <a class="card" href="/dashboard">
            <h2>Dashboard</h2>
            <p>View all boxes and workflow status.</p>
        </a>

        <a class="card" href="/scanner">
            <h2>Scanner</h2>
            <p>View boxes waiting to be scanned.</p>
        </a>

        <a class="card" href="/indexing">
            <h2>Indexing</h2>
            <p>View boxes ready for indexing.</p>
        </a>

        <a class="card" href="/qa">
            <h2>QA</h2>
            <p>Review and approve indexed boxes.</p>
        </a>

        <a class="card" href="/delivery">
            <h2>Delivery</h2>
            <p>Mark approved boxes as delivered.</p>
        </a>
    </div>
</body>
</html>







nAAAAA


<nav style="background:#111827; padding:14px 20px; margin:-30px -30px 30px -30px;">
    <a href="/" style="color:white; text-decoration:none; margin-right:20px; font-weight:bold;">Home</a>
    <a href="/dashboard" style="color:white; text-decoration:none; margin-right:20px;">Dashboard</a>
    <a href="/scanner" style="color:white; text-decoration:none; margin-right:20px;">Scanner</a>
    <a href="/indexing" style="color:white; text-decoration:none; margin-right:20px;">Indexing</a>
    <a href="/qa" style="color:white; text-decoration:none; margin-right:20px;">QA</a>
    <a href="/delivery" style="color:white; text-decoration:none;">Delivery</a>
</nav>



    
