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
  } from "carbon-components-svelte";
  import { Grid, Row, Column } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";
  import{onMount} from 'svelte';

  
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

  onMount(async() => {
    const response = await fetch('http://localhost:8080/api/v1/orders/');
    orders = await response.json()
    //console.log(orders)

    table_orders = orders;
    
    table_orders.forEach((element) => {
        ['orderLineItems', 'taxTotal', 'customerStreet', 'customerZipcode', 'date', 'netTotal', 'customerCity', 'customerCountry'].forEach(e => delete element[e]);
        element.id = Math.random();
    });

    
    
    console.log(table_orders)

    rows = rows.concat(table_orders)
    console.log(rows)

  })

  let open = false;


</script>



<br /><br /><br /><br /><br /><br /><br /><br />

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

    <Column lg={4} />
  </Row>



</Grid>





<ComposedModal bind:open on:submit={() => (open = false)}>
  <ModalHeader label="Customer" title="add Customer" />
  <ModalBody hasForm>

    <TextInput labelText="customerID" placeholder="1234567891" required />
    <TextInput labelText="customerFirstname" placeholder="Max" required />
    <TextInput labelText="customerLastname"  placeholder="Mustermann" required />
    <TextInput labelText="customerEmail" type="email"  placeholder="max@gmx.at" required />
    <TextInput labelText="customerStreet"  placeholder="Sesamstraße 2" required />
    <TextInput labelText="customerZipcode"  placeholder="6460" required />
    <TextInput labelText="customerCity"  placeholder="Imst" required />
    <TextInput labelText="customerCountry"  placeholder="Österreich" required />
    <br>
    <p>Cart Item</p>
    <TextInput inline labelText="productNumber" placeholder="1234567891" />
    <TextInput inline labelText="productName" placeholder="IPhone 6" type="number" />
    <TextInput inline labelText="priceNet" placeholder="100" />
    <TextInput inline labelText="tax" placeholder="20" />
    <TextInput inline labelText="amount" placeholder="120" />

    
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
