Bus Ticketing System
This project focuses on WPF and a database in C#. Users can work with C#'s built-in classes and methods to create a functional Bus Ticketing System.   

Booking Tickets:  
Users can book new bus tickets by providing passenger information, destination, departure date, and price. 
Each ticket should have a unique ID assigned automatically. 

Editing Tickets:  Users can edit and update existing ticket details, including passenger name, destination, departure date, and ticket price. 

Cancelling Tickets:  Users can cancel booked tickets, removing them from the system. 

Viewing Booked Tickets:  Users can view a list of all booked tickets in a DataGrid, which displays ticket details. 

Database Interaction:  The application interacts with an external SQL Server database to store and retrieve ticket data. 

How to run and use the application:

Running the Application:

Open the project in Visual Studio:
Open Visual Studio.
Open the solution file for the Bus Ticketing System project (.sln file).

Set up a database connection:
Open the MainForm.cs file.
Locate the ConnectionString variable and add an SQL Server connection string.

Build and Run:
Build the solution by clicking on the "Build" menu and selecting "Build Solution".
Run the application by clicking on the "Start" button.  

Using the Bus Ticketing System:

Booking a new ticket:
Enter the passenger name, destination, departure date, and ticket price in the respective text boxes.
Click the "Book" button to add the new ticket to the system.

Editing an existing ticket:
Select a ticket from the DataGrid by clicking on the corresponding row.
Make changes to the passenger name, destination, departure date, or ticket price in the text boxes.
Click the "Edit" button to update the ticket information.

Cancelling a booked ticket:
Select a ticket from the DataGrid by clicking on the corresponding row.
Click the "Cancel" button to remove the selected ticket from the system.

Viewing booked tickets:
Click the "Refresh" button to load and display all booked tickets in the DataGrid.

Designing the User Interface (UI):
Create a new Windows Forms Application in Visual Studio.
Design the form with the following elements:
DataGridView (named dataGridViewTickets) to display your booked tickets.
TextBoxes for passenger name (txtPassengerName), destination (txtDestination), and ticket price (txtTicketPrice).
DateTimePicker for selecting the departure date (dateTimePickerDepartureDate).
Buttons for booking (btnBook), editing (btnEdit), cancelling (btnCancel), and refreshing (btnRefresh).

Using an external SQL Server Database:
Open SQL Server Management Studio (SSMS).
Create a new database named "BusTicketDB."
Inside "BusTicketDB," create a table named "BookedTickets" with the following specific columns:
USE BusTicketDB;

CREATE TABLE BookedTickets
(
    TicketID INT PRIMARY KEY IDENTITY,
    PassengerName VARCHAR(50),
    Destination VARCHAR(50),
    DepartureDate DATE,
    TicketPrice DECIMAL
);

Bus Ticketing application
Write the code for the event handlers to interact with the external SQL Server database. 
Use the following code to the code-behind file. For my example, my file name is PrimaryForm.cs:

using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace BusTicketingSystem
{
    public partial class PrimaryForm: Form
    {
        private const string ConnectionString = "GABIRAKGOTSOKA\MSSQLSERVER01";

        public PublicForm()
        {
            InitializeComponent();
            LoadTickets();
        }

        private void LoadTickets()
        {
            using (SqlConnection connection = new SqlConnection(ConnectionString))
            {
                connection.Open();
                string query = "SELECT * FROM BookedTickets";
                SqlDataAdapter adapter = new SqlDataAdapter(query, connection);
                DataTable dataTable = new DataTable();
                adapter.Fill(dataTable);
                dataGridViewTickets.DataSource = dataTable;
            }
        }

        private void btnBook_Click(object sender, EventArgs e)
        {
            try
            {
                using (SqlConnection connection = new SqlConnection(ConnectionString))
                {
                    connection.Open();
                    string query = "INSERT INTO BookedTickets (PassengerName, Destination, DepartureDate, TicketPrice) " +
                                   "VALUES (@PassengerName, @Destination, @DepartureDate, @TicketPrice)";
                    SqlCommand command = new SqlCommand(query, connection);

                    command.Parameters.AddWithValue("@PassengerName", txtPassengerName.Text);
                    command.Parameters.AddWithValue("@Destination", txtDestination.Text);
                    command.Parameters.AddWithValue("@DepartureDate", dateTimePickerDepartureDate.Value);
                    command.Parameters.AddWithValue("@TicketPrice", decimal.Parse(txtTicketPrice.Text));

                    command.ExecuteNonQuery();
                }

                LoadTickets();
                ClearInputs();
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnEdit_Click(object sender, EventArgs e)
        {
            // Similar logic for editing a ticket
        }

        private void btnCancel_Click(object sender, EventArgs e)
        {
            // Similar logic for cancelling a ticket
        }

        private void btnRefresh_Click(object sender, EventArgs e)
        {
            LoadTickets();
        }

        private void ClearInputs()
        {
            txtPassengerName.Clear();
            txtDestination.Clear();
            txtTicketPrice.Clear();
            dateTimePickerDepartureDate.Value = DateTime.Now;
        }
    }
}

Exception Handling
Basic exception handling is implemented in the given code to find and display errors.

Future improvements:
To enhance the application, I would consider adding validation for user inputs, improving the user interface, and implementing additional features such as user authentication and authorization.


