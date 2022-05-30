<script>
  import {
    DataTable,
    Toolbar,
    ToolbarContent,
    ToolbarSearch,
    ToolbarMenu,
    ToolbarMenuItem,
    ToolbarBatchActions,
    Button,
    ComposedModal,
    ModalHeader,
    ModalBody,
    ModalFooter,
    Form
  } from "carbon-components-svelte";
  import { Grid, Row, Column } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";
  import{onMount} from 'svelte';
 import { Loading } from "carbon-components-svelte";
 import { TextArea } from "carbon-components-svelte";
  
  import {
    FluidForm,
    TextInput,
    PasswordInput,
  } from "carbon-components-svelte";

  
  const headers = [
    {key: "orderID", value: "OrderID" },
    {key: "customerID", value: "CustomerID"},
    {key: "customerFirstname", value: "Firstname"},
    {key: "customerLastname", value: " Lastname"},
    {key: "customerEmail", value: "Email"},
    {key: "grossTotal", value: "Total"},
    {key: "state", value: "State"}

  ];

  let rows = [];
  let orders = []
  let table_orders = []
  const url = "http://localhost:8080";

  let open = false;
  let active = false;

  onMount(async() => {
      getAllOrdersForTable();
  })


 async function getAllOrdersForTable() {
    active = true;
    const response = await fetch(url + '/api/v1/orders/');
    orders = await response.json()
    //console.log(orders)

    table_orders = orders;
    
    table_orders.forEach((element) => {
        ['orderLineItems', 'taxTotal', 'customerStreet', 'customerZipcode', 'date', 'netTotal', 'customerCity', 'customerCountry'].forEach(e => delete element[e]);
        element.id = Math.random();
    });

    //console.log(table_orders)
    rows = [];
    console.log(rows)
    rows = rows.concat(table_orders)
    active = false;
    //console.log(rows)
  }

  var	item = {
    productNumber: "stringstri",
    productName: "string",
    priceNet: 0,
    tax: 0,
    amount: 0
  };
  
  let customerID;
  let customerFirstname;
  let customerLastname;
  let customerEmail;
  let customerStreet;
  let customerZipcode;
  let customerCity;
  let customerCountry;
  let cartItemsBind = JSON.stringify(item,null,'\t');


  async function handleSubmit() {
    open = false
    placeOrder();
  
  }

  async function placeOrder() {
    let cartItems = JSON.parse(cartItemsBind)

    const res = await fetch(url + '/api/v1/orders/', {
			method: 'POST',
			body: JSON.stringify({
				customerID,
        customerFirstname,
        customerLastname,
        customerEmail,
        customerStreet,
        customerZipcode,
        customerCity,
        customerCountry,
        cartItems

			}),
      headers: {
        "content-type": "application/json"
      }
		})
		
		const json = await res.json()
		result = JSON.stringify(json)
    console.log(result)
	}



  



</script>



<br /><br /><br /><br /><br /><br /><br /><br />

<Loading active={active}/>

<Grid fullWidth>
  <Row>
    <Column lg={4} />
    <Column lg={8}>
      <DataTable 
      title="Orders"
      description="Hier kannst du Orders einsehen"
      {headers} {rows}
      >

    </DataTable>


    </Column>
    <Column lg={4} />
  </Row>


  <br><br>
  <Row>
    <Column lg={4} />

    <Column>
      <ClickableTile on:click={() => (open = true)}>place Order</ClickableTile>
    </Column>

    <Column>
      <ClickableTile on:click={() => getAllOrdersForTable()}>reload Table</ClickableTile>
    </Column>


    <Column lg={4} />
  </Row>



</Grid>





<ComposedModal bind:open on:submit={() => handleSubmit()}>
  <ModalHeader label="Customer" title="place Order" />
  <ModalBody hasForm>

    <TextInput bind:value={customerID} labelText="customerID" placeholder="1234567891" required />
    <TextInput bind:value={customerFirstname} labelText="customerFirstname" placeholder="Max" required />
    <TextInput bind:value={customerLastname} labelText="customerLastname"  placeholder="Mustermann" required />
    <TextInput bind:value={customerEmail} labelText="customerEmail" type="email"  placeholder="max@gmx.at" required />
    <TextInput bind:value={customerStreet} labelText="customerStreet"  placeholder="Sesamstraße 2" required />
    <TextInput bind:value={customerZipcode} labelText="customerZipcode"  placeholder="6460" required />
    <TextInput bind:value={customerCity} labelText="customerCity"  placeholder="Imst" required />
    <TextInput bind:value={customerCountry} labelText="customerCountry"  placeholder="Österreich" required />
    <br>
    <!--
    <p>Cart Item</p>
    <TextInput inline labelText="productNumber" placeholder="1234567891" />
    <TextInput inline labelText="productName" placeholder="IPhone 6" type="number" />
    <TextInput inline labelText="priceNet" placeholder="100" />
    <TextInput inline labelText="tax" placeholder="20" />
    <TextInput inline labelText="amount" placeholder="120" />
    --> 
    
    <TextArea

      rows={20}
      labelText="App CartItems"
      bind:value={cartItemsBind}
/>

    
  </ModalBody>

  <ModalFooter primaryButtonText="Proceed" type="submit"  />

</ComposedModal>




<style>
  h1 {
    color: #ff3e00;
    font-size: 4em;
    font-weight: 400;
  }
</style>
