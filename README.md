```cs

using System;
using System.Collections.Generic;

using Newtonsoft.Json;
using System.Text.Json;
using System.Text.Json.Serialization;

public class Employee
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

public class Program
{
    public static void Main()
    {
        List<Employee> employees = new List<Employee> 
        {
            new Employee 
            {
                FirstName = "Mark", 
                LastName = null ,
            }
        };

        // Newtonsoft
        { 
            // Newtonsoft (without NullValueHandling)
            var newtonsoftString = JsonConvert.SerializeObject(employees);
            Console.WriteLine("Newtonsoft (without NullValueHandling): " + newtonsoftString); // will print "Newtonsoft (without NullValueHandling): [{"FirstName":"Mark","LastName":null}]"
            var deserializedList = JsonConvert.DeserializeObject<List<Employee>>(newtonsoftString);


            // Newtonsoft (NullValueHandling)
            var settings = new JsonSerializerSettings { NullValueHandling = NullValueHandling.Ignore };
            newtonsoftString = JsonConvert.SerializeObject(employees, settings);
            Console.WriteLine("Newtonsoft (with NullValueHandling): " + newtonsoftString); // will print "Newtonsoft (with NullValueHandling): [{"FirstName":"Mark"}]"
            deserializedList = JsonConvert.DeserializeObject<List<Employee>>(newtonsoftString, settings);

        }

        // System.Text.Json
        {
            // System.Text.Json (without NullValueHandling)
            var jsonString = System.Text.Json.JsonSerializer.Serialize(employees);
            Console.WriteLine("System.Text.Json (without NullValueHandling): " + jsonString); // will print "[{"FirstName":"Mark","LastName":null}]"
            var deserializedList = System.Text.Json.JsonSerializer.Deserialize<List<Employee>>(jsonString);

            // System.Text.Json (NullValueHandling)
            var options = new JsonSerializerOptions
            {
                DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
            };

            jsonString = System.Text.Json.JsonSerializer.Serialize(employees, options);
            Console.WriteLine("System.Text.Json (with NullValueHandling): " + jsonString); // will print "System.Text.Json (with NullValueHandling): [{"FirstName":"Mark"}]"
            deserializedList = System.Text.Json.JsonSerializer.Deserialize<List<Employee>>(jsonString, options);
        }

        Console.ReadLine();
    }
}

```
