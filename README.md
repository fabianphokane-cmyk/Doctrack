<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DocTrack Dashboard</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            margin: 0;
            padding: 30px;
        }

        h1 {
            margin-bottom: 20px;
            color: #222;
        }

        .stats {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            min-width: 200px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .card h2 {
            margin: 0;
            font-size: 28px;
            color: #111;
        }

        .card p {
            margin: 8px 0 0;
            color: #666;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        th, td {
            padding: 14px;
            text-align: left;
            border-bottom: 1px solid #eee;
        }

        th {
            background: #111827;
            color: white;
        }

        tr:hover {
            background: #f9fafb;
        }

        .status {
            padding: 6px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
            background: #e5e7eb;
            color: #111827;
            display: inline-block;
        }
    </style> 
<head>
    <style>
        .status {
            display: inline-block;
            padding: 5px 10px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
        }

        .received { background: #ccc; color: #333; }
        .ready_for_index { background: #3498db; color: white; }
        .ready_for_qa { background: #f39c12; color: white; }
        .qa_passed { background: #2ecc71; color: white; }
        .delivered { background: #27ae60; color: white; }
        .indexing { background: #e74c3c; color: white; }
    </style>
</head>
<body>
    <h1>DocTrack Dashboard</h1>
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

    <div class="stats">
        <div class="card">
            <h2>{{ total_boxes }}</h2>
            <p>Total Boxes</p>
        </div>

        <div class="card">
            <h2>{{ ready_for_index }}</h2>
            <p>Ready for Index</p>
        </div>

        <div class="card">
            <h2>{{ ready_for_qa }}</h2>
            <p>Ready for QA</p>
        </div>

        <div class="card">
            <h2>{{ delivered }}</h2>
            <p>Delivered</p>
        </div>
    </div>

    <table>
        <thead>
            <tr>
                <th>Box ID</th>
                <th>Client</th>
                <th>Project</th>
                <th>Description</th>
                <th>Status</th>
                <th>Actions</th>
            </tr>
        </thead>
       <tbody>
    {% for box in boxes %}
    <tr>
        <td>{{ box.box_id }}</td>
        <td>{{ box.client }}</td>
        <td>{{ box.project }}</td>
        <td>{{ box.description }}</td>
 <td>
    <span class="status {{ box.status }}">{{ box.status }}</span>
</td>
<td>
    <a href="/boxes/{{ box.box_id }}/history/view">View History</a><br><br>

    {% if box.status == "ready_for_qa" %}
    <form action="/dashboard/qa_approve/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Approve</button>
    </form>
    <form action="/dashboard/qa_reject/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Reject</button>
    </form>
    {% elif box.status == "qa_passed" or box.status == "ready_for_delivery" %}
    <form action="/dashboard/delivered/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Mark Delivered</button>
    </form>
    {% else %}
    -
    {% endif %}
</td>
</tr>
{% endfor %} 
</tbody>
