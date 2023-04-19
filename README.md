# Convert WooCommerce dropdown attribute to radio button

![WC Dropdown to Radio.](https://prnt.sc/mE-wCOjVzwgC)

```
// modify single product select dropdown to input radio 
add_action( 'woocommerce_variable_add_to_cart', function() {

    add_action( 'wp_print_footer_scripts', function() { ?>
        <script type="text/javascript">

        // DOM Loaded
        document.addEventListener( 'DOMContentLoaded', function() {
            // Get Variation Pricing Data
            var variations_form = document.querySelector( 'form.variations_form' );
            var data            = variations_form.getAttribute( 'data-product_variations' );
            data                = JSON.parse( data );

            // Loop Drop Downs
            document.querySelectorAll( 'table.variations select' )
                .forEach( function( select ) {

                // Loop Drop Down Options
                select.querySelectorAll( 'option' )
                    .forEach( function( option ) {

                    // Skip Empty
                    if( ! option.value ) {
                        return;
                    }

                    // Get Pricing For This Option
                    var pricing = '';
                    data.forEach( function( row ) {
                        if( row.attributes[select.name] == option.value ) {
                            pricing = row.price_html;
                        }
                    } );

                    // Create Radio
                    var radio = document.createElement( 'input' );
                        radio.type = 'radio';
                        radio.name = select.name;
                        radio.value = option.value;
                        radio.checked = option.selected;
                    var label = document.createElement( 'label' );
                        label.appendChild( radio );
                        // label.appendChild( document.createTextNode( ' ' + option.text + ' ' ) );
                        label.classList.add( option.value )

                    // var span = document.createElement( 'span' );
                        // span.innerHTML = pricing;
                        // label.appendChild( span );

                    var div = document.createElement( 'div' );
                        div.appendChild( label );

                    // Insert Radio
                    select.closest( 'td' ).appendChild( div );

                    // Handle Clicking
                    radio.addEventListener( 'click', function( event ) {
                        select.value = radio.value;
                        jQuery( select ).trigger( 'change' );
                    } );

                } ); // End Drop Down Options Loop

                // Hide Drop Down
                select.style.display = 'none';

            } ); // End Drop Downs Loop
        } ); // End Document Loaded

        </script>
        <?php

    } );

} );
```
