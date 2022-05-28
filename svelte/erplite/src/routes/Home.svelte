<script>
  import { Button } from "carbon-components-svelte";

import {
  Header,
  HeaderNav,
  HeaderNavItem,
  HeaderNavMenu,
  SkipToContent,
  Content,
  Grid,
  Row,
  Column,
} from "carbon-components-svelte";
import { ClickableTile } from "carbon-components-svelte";


  
</script>


<Content>
    <Grid>
      <Row>
        <Column>
          <h1>Welcome to ErpLite</h1>
        </Column>
      </Row>
    </Grid>
  </Content>
  
  <Grid>
    <Row>
      <Column>
        <ClickableTile href="/#/order">Orders</ClickableTile>
      </Column>
      <Column>
        <ClickableTile href="/#/customer">Customer</ClickableTile>
      </Column>
      <Column><ClickableTile href="/">etc</ClickableTile></Column>
      <Column><ClickableTile href="/">etc</ClickableTile></Column>
    </Row>
  </Grid>


  