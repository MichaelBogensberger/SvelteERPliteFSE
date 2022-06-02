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
    Form,
  } from "carbon-components-svelte";
  import { Grid, Row, Column } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";
  import { onMount } from "svelte";
  import { Loading } from "carbon-components-svelte";
  import { TextArea } from "carbon-components-svelte";

  import {
    FluidForm,
    TextInput,
    PasswordInput,
  } from "carbon-components-svelte";

  const headers = [
    { key: "orderID", value: "OrderID" },
    { key: "name", value: "Name" },
    { key: "street", value: "Street" },
    { key: "itemID", value: "Item ID" },
    { key: "productName", value: "Product Name" },
    { key: "packed", value: "Packed" },
  ];

  /*
  let rows = [
    {
      id: "213",
      orderID: "OA1234312",
      name: "Michael Bogensberger",
      street: "Sesamstraße 2",
      itemID: "12354332",
      productName: "IPhone",
      packed: false,
    },
  ];*/

  let rows = []



  const url = "http://localhost:8080";

  let active = false;

  onMount(async () => {
    getPackings();
  });

  //let packings = [];

  async function getPackings() {
    active = true;
    const response = await fetch(url + "/stock/packings")
      .then((response) => response.json())
      .then((data) => {
        //console.log(data)

        rows = [];
        let index;
        let temp = {}

        for (const entry of data) {
          index = data.indexOf(entry);
          console.log(entry);
          //console.log(index);

          for(const item of entry.packingItemList) {
            console.log(item)
            
            temp = {
                id: Math.random(),
                orderID: entry.orderId,
                name: entry.deliveryData.name,
                street: entry.deliveryData.street,
                itemID: item.id,
                productName: item.productName,
                packed: item.packed,
            }

            rows.push(temp)
            rows = rows;

          }



        }
      });

    //packings = await response.json();

    //console.log(packings)

    active = false;
  }
</script>

<br /><br /><br /><br /><br /><br /><br /><br />

<Loading {active} />

<Grid fullWidth>
  <Row>
    <Column lg={4} />
    <Column lg={8}>
      <DataTable
        title="Orders"
        description="Hier kannst du Orders einsehen"
        {headers}
        {rows}
      />
    </Column>
    <Column lg={4} />
  </Row>

  <br /><br />
  <Row>
    <Column lg={4} />

    <Column>
      <ClickableTile>zum versand makieren</ClickableTile>
    </Column>

    <Column lg={4} />
  </Row>
</Grid>

<style>
  h1 {
    color: #ff3e00;
    font-size: 4em;
    font-weight: 400;
  }
</style>
