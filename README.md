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
            display: inline-block;
            padding: 5px 10px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            background: #e5e7eb;
            color: #111827;
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




    </table>
</body>
</html>
