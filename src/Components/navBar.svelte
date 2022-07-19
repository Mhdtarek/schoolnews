<script>
    import {
      Navbar,
      NavbarBrand,
      Nav,
      NavItem,
      NavLink,
      Dropdown,
      DropdownItem,
      DropdownMenu,
      DropdownToggle
    } from 'sveltestrap';
  import {userPhotoURL, userDisplayName, userCreation} from './Auth.svelte'
  </script>
  
<Navbar color="light" light expand="md">
  <NavbarBrand href="#">bbrr</NavbarBrand>
    <Nav class="ms-auto" navbar>
      <NavItem>
        <Dropdown direction="left" nav setActiveFromChild>
          <DropdownToggle nav class="nav-link"><img src={$userPhotoURL} alt="user" height="32" width="32"></DropdownToggle>
          <DropdownMenu dark>
            <DropdownItem header>{$userDisplayName}</DropdownItem>
            <DropdownItem href="#" active>Lancelot</DropdownItem>
            <DropdownItem href="#">Link</DropdownItem>
            <DropdownItem href="#">Secret</DropdownItem>
            <DropdownItem href="#">Chimp</DropdownItem>
          </DropdownMenu>
        </Dropdown>
      </NavItem>
      <NavItem>
        
      </NavItem>

    </Nav>
</Navbar>