# Existing content and added record (Ultra Super Maximus)
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json; charset=utf-8
Date: Sat, 04 Jul 2026 13:34:06 GMT
Server: Kestrel
Transfer-Encoding: chunked

[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 4,
    "name": "Ultra Super Maximus",
    "isGlutenFree": false
  }
]

# Summary function
void GenerateSalesSummary(IEnumerable<string> salesFiles, double salesTotal, string outputFolder)
{
    StringBuilder report = new();

    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($"Total Sales: {salesTotal:C}");
    report.AppendLine();
    report.AppendLine("Details:");

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);

        SalesData? data = JsonConvert.DeserializeObject<SalesData>(salesJson);

        double total = data?.Total ?? 0;

        string folder = Path.GetFileName(Path.GetDirectoryName(file)!);
        string relativePath = Path.Combine(folder, Path.GetFileName(file));

        report.AppendLine(
            $"{relativePath}: {total:C}"
        );

        string reportPath = Path.Combine(outputFolder, "totals.txt");

        File.WriteAllText(reportPath, report.ToString());
    }
}