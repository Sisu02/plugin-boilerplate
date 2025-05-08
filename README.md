add_filter( 'woocommerce_shipping_calculator_enable_address', '__return_true' );
woocommerce_form_field( 'calc_shipping_address', array(
    'type'        => 'text',
    'class'       => array( 'form-row-wide' ),
    'label'       => __( 'Street address', 'woocommerce' ),
    'placeholder' => __( 'House number and street name', 'woocommerce' ),
), $address );

yourtheme/woocommerce/cart/shipping-calculator.php
