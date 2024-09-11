# FizzBuzz

## Part 1

```C#
public class Program
{
    public static void Main()
    {
        Console.WriteLine("Fizz Buzz\n");         
        FizzBuzz();
    }
    
    public static void FizzBuzz() {
        for (int currentNumber = 1; currentNumber <= 100; currentNumber++) {
            string result = FizzBuzzGame(currentNumber);
            Console.WriteLine(result);
            SaveFizzBuzzResult(result);
        }
    }
    
    public static string FizzBuzzGame(int currentNumber) {
        if (currentNumber % 3 == 0 && currentNumber % 5 == 0) 
                        return "FizzBuzz";
        if (currentNumber % 3 == 0) 
                        return "Fizz";
        if (currentNumber % 5 == 0) 
                        return "Buzz";
        return currentNumber.ToString();
    }
    
    public static void SaveFizzBuzzResult(string result) {
        string connectionString = ConfigurationManager.ConnectionString["DefaultConnection"];
        
        using(sqlConnection connection = new sqlConnection(connectionString)) {
        
            connection.Open();

            using (sqlCommand command = new sqlCommand("RecordFizzBuzzResults", connection)) {
            {
                command.CommandType = CommandType.StoredProcedure;
                command.Parameters.AddWithValue("@Result", result);
                command.ExecuteNonQuery();
            }
            
            connection.Close()
        }
    }

}
```

-- Provide the SQL to create a table to handle the FizzBuzz results
-- The table should include an unique ID column and a results column

```SQL
create table FizzBuzzGameResults (
    Id int primary key identity(1,1)
    Result varchar(30) not null
)
```

-- provide the SQL to a stored procedure to store the results of the Fizz

```SQL
create procedure LogFizzBuzzResult
    @GameResult varchar (30)
as
begin
    insert into FizzBuzzGameResults (GameResult) values (@GameResult);
end;
```

-- create a function called by the FizzBuzz program to execute the stored procedure by passing in the results to a stored procedure.

```SQL
create function RecordFizzBuzzResults (@Result varchar(30)
return int
as 
begin
    declare @ResultStatus int;
    execute @ResultStatus = LogFizzBuzzResult @Result
end;
```
