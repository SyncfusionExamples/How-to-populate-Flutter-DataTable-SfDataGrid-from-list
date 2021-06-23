# How to populate Flutter DataTable SfDataGrid from list

The Flutter DataTable requires the collection of the DataGridRow to show the data. The following steps explains how to load the data from the list collection to the Flutter DataTable.
## STEP 1
Import the following library in the flutter application.
```xml
import 'package:syncfusion_flutter_datagrid/datagrid.dart';
```

## STEP 2

The SfDataGrid is dependent upon the data. Create a simple data source for the SfDataGrid.
Here, create an employee data source. 

```xml
class Employee {
  Employee(this.id, this.name, this.designation, this.salary);

  final int id;

  final String name;

  final String designation;

  final int salary;
}
```

Create the collection of employee data with the required number of data objects. Here, the method which is used to populate the data objects is initialized in initState(). The DataGridSource object is expected to be long-lived, not re-created with each build.

```xml
List<Employee> employees = <Employee>[];

late EmployeeDataSource employeeDataSource;

@override
void initState() {
  super.initState();
  employees= getEmployees();
  employeeDataSource = EmployeeDataSource(employees: employees);
}

 List<Employee> getEmployees() {
  return[
  Employee(10001, 'James', 'Project Lead', 20000),
  Employee(10002, 'Kathryn', 'Manager', 30000),
  Employee(10003, 'Lara', 'Developer', 15000),
  Employee(10004, 'Michael', 'Designer', 15000),
  Employee(10005, 'Martin', 'Developer', 15000),
  Employee(10006, 'Newberry', 'Developer', 15000),
  Employee(10007, 'Balnc', 'Developer', 15000),
  Employee(10008, 'Perry', 'Developer', 15000),
  Employee(10009, 'Gable', 'Developer', 15000),
  Employee(10010, 'Grimes', 'Developer', 15000)
  ];
}
```

## STEP 3: 

The DataGridSource is used to obtain the row data for the SfDataGrid. So, create 
the data source class and override the rows and the buildRow APIs in it.

```xml
class EmployeeDataSource extends DataGridSource {
  EmployeeDataSource({List<Employee> employees}) {
     _employees = employees
        .map<DataGridRow>((e) => DataGridRow(cells: [
              DataGridCell<int>(columnName: 'id', value: e.id),
              DataGridCell<String>(columnName: 'name', value: e.name),
              DataGridCell<String>(
                  columnName: 'designation', value: e.designation),
              DataGridCell<int>(columnName: 'salary', value: e.salary),
            ]))
        .toList();
  }

  List<DataGridRow>  _employees = [];

  @override
  List<DataGridRow> get rows =>  _employees;

  @override
  DataGridRowAdapter? buildRow(DataGridRow row) {
    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((dataGridCell) {
      return Container(
        alignment: (dataGridCell.columnName == 'id' || dataGridCell.columnName == 'salary')
            ? Alignment.centerRight
            : Alignment.centerLeft,
        padding: EdgeInsets.all(16.0),
        child: Text(dataGridCell.value.toString()),
      );
    }).toList());
  }
}
```

## STEP 4

 Create an instance of the DataGridSource and set this object to the source property of the SfDataGrid.

```xml
@override
Widget build(BuildContext context) {
  return Scaffold(
      appBar: AppBar(
        title: Text('Syncfusion DataGrid'),
      ),
      body: Center(
        child: Expanded(
          child: SfDataGrid(
            source: _employeeDataSource,
          ),
        ),
      ));
```

## STEP 5

 Finally, add the column collection to the columns property. 
 Here give an id, name, designation, and salary columns to the columns property.

```xml
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Syncfusion Flutter DataGrid'),
    ),
    body: SfDataGrid(
      source: employeeDataSource,
      columns: <GridColumn>[
        GridTextColumn(
            columnName: 'id',
            label: Container(
                padding: EdgeInsets.all(16.0),
                alignment: Alignment.centerRight,
                child: Text(
                  'ID',
                ))),
        GridTextColumn(
            columnName: 'name',
            label: Container(
                padding: EdgeInsets.all(16.0),
                alignment: Alignment.centerLeft,
                child: Text('Name'))),
        GridTextColumn(
            columnName: 'designation',
            width: 120,
            label: Container(
                padding: EdgeInsets.all(16.0),
                alignment: Alignment.centerLeft,
                child: Text('Designation'))),
        GridTextColumn(
            columnName: 'salary',
            label: Container(
                padding: EdgeInsets.all(16.0),
                alignment: Alignment.centerRight,
                child: Text('Salary'))),
      ],
    ),
  );
```
