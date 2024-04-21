import requests
from ipywidgets import widgets, Layout, Button, Text, VBox, Label, Output
from IPython.display import display, clear_output

# Function to send meter reading request
def send_meter_reading_request(ip, port, serial_number, customer_id):
    url = f"http://{ip}:{port}/api/Dashboard/MeterReading"
    payload = {"MeterSerialNumber": serial_number, "CustomerID": customer_id}
    headers = {'Content-Type': 'application/json'}
    try:
        response = requests.post(url, json=payload, headers=headers)
        return response.text
    except Exception as e:
        return str(e)

# Function to send customer validation request
def send_customer_validation_request(ip, port, customer_id):
    url = f"http://{ip}:{port}/api/Dashboard/CustomerValidation"
    payload = {"CustomerID": customer_id}
    headers = {'Content-Type': 'application/json'}
    try:
        response = requests.post(url, json=payload, headers=headers)
        return response.text
    except Exception as e:
        return str(e)

# Creating UI components for a more organized and styled layout
ip_address_widget = Text(value='', placeholder='Enter IP Address', description='IP Address:', layout=Layout(width='95%'))
port_widget = Text(value='', placeholder='Enter Port', description='Port:', layout=Layout(width='95%'))
serial_number_widget = Text(value='', placeholder='Enter Serial Number', description='Serial Number:', layout=Layout(width='95%'))
customer_id_widget = Text(value='', placeholder='Enter Customer ID', description='Customer ID:', layout=Layout(width='95%'))
output_area = Output()

send_meter_button = Button(description="Send Meter Reading Request", button_style='success', layout=Layout(width='95%', height='40px'))
send_customer_button = Button(description="Send Customer Validation Request", button_style='success', layout=Layout(width='95%', height='40px'))

def on_meter_button_clicked(b):
    with output_area:
        clear_output()
        print("Sending Meter Reading Request...")
        response = send_meter_reading_request(ip_address_widget.value, port_widget.value, serial_number_widget.value, customer_id_widget.value)
        print("Response:", response)

def on_customer_button_clicked(b):
    with output_area:
        clear_output()
        print("Sending Customer Validation Request...")
        response = send_customer_validation_request(ip_address_widget.value, port_widget.value, customer_id_widget.value)
        print("Response:", response)

send_meter_button.on_click(on_meter_button_clicked)
send_customer_button.on_click(on_customer_button_clicked)

# Grouping widgets by functionality
meter_reading_section = VBox([Label('Meter Reading API'), ip_address_widget, port_widget, serial_number_widget, customer_id_widget, send_meter_button, output_area],
                             layout=Layout(border='2px solid blue', padding='10px', margin='10px'))

customer_validation_section = VBox([Label('Customer Validation API'), ip_address_widget, port_widget, customer_id_widget, send_customer_button, output_area],
                                   layout=Layout(border='2px solid green', padding='10px', margin='10px'))

# Display the UI
display(meter_reading_section, customer_validation_section)
