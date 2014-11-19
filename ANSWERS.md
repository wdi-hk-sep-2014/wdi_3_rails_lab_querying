# Tow Lot Querying Lab

The [tow lot app](https://github.com/ga-wdi-boston/wdi_3_rails_lab_migrations) you built earlier has been in use for a few weeks, and the owner has some questions about the data that is now entered into it. Run `rake db:setup` to create the database and load this data, then open a `rails console` and write a *single* ActiveRecord query to achieve each of the following objectives. Record your finished queries in a separate file.

**Note:** "Vehicle" here only refers to records in the `vehicles` table (i.e. not tow trucks).

1) Get a list of all tow trucks.

```
TowTruck.all
```

2) Get the vehicle with ID 3.

```
Vehicle.find 3
```

3) Get the vehicle with VIN D0985DF1593A350A4.

```
Vehicle.find_by_vin "D0985DF1593A350A4"
```

4) Get a list of all vehicles sorted by acquisition date.

```
Vehicle.order(acquired_at: :asc)
```

5) Get a list of all silver vehicles, ordered alphabetically by make.

```
Vehicle.where(color: "silver").order(make: :asc)
```

6) Get a list of all red Honda vehicles that are cars.

```
Vehicle.where(color: "red", make: "Honda", category: "car")
```

7) Get a count of all vehicles that are motorcycles.

```
Vehicle.where(category: "motorcycle").count
#=> 5
```

8) Get a count of vehicles that are currently on the lot (i.e. not released).

```
Vehicle.where(released_at: nil).count
#=> 6
```

9) Get a list of all tow trucks whose mileage is over 200,000.

```
TowTruck.where("mileage > 200000")
```

10) Get a list of all tow trucks whose last service was more than 8 months ago.

```
TowTruck.where("last_service_at < ?", 8.month.ago)
```

11) Get the average tow fee across all vehicles.

```
Vehicle.average(:fee).to_f
```

12) Get the top 3 vehicles with the highest tow fees.

```
Vehicle.order(fee: :asc).limit(3)
```

13) Get all vehicles that have a fee assessed which has not been paid.

```
Vehicle.where.not(fee: nil).where(is_paid: false)
```

14) Get a list of all released vehicles, sorted by most recently released.

```
Vehicle.where.not(released_at: nil).order(released_at: :desc)
```

15) Get a list of all vehicles that have notes.

```
Vehicle.where.not(notes: "")
```

16) Get the VINs of all vehicles manufactured before the year 2000.

```
Vehicle.where("year < 2000").pluck(:vin)
```

17) Get the make, model, and year of all silver vehicles, ordered by year.

```
Vehicle.where(color: "silver").order(year: :asc).pluck(:make, :model, :year)
```

18) Get the total income from all vehicles (i.e. sum of all fees that are paid).

```
Vehicle.where(is_paid: true).sum(:fee).to_f
```


## Use `OR` case in `WHERE` clause

```
Option 1:

Vehicle.where("make = 'Honda' OR make = 'Toyota'")

Option 2:

Vehicle.where(make: ['Honda', 'Toyota'])

Option 3:
Reference to http://stackoverflow.com/a/13754977/2332394

vehicles = Vehicle.arel_table
Vehicle.where(vehicles[:make].eq("Honda").or(vehicles[:make].eq("Toyota"))
```