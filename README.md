add_filter( 'woocommerce_shipping_calculator_enable_address', '__return_true' );
woocommerce_form_field( 'calc_shipping_address', array(
    'type'        => 'text',
    'class'       => array( 'form-row-wide' ),
    'label'       => __( 'Street address', 'woocommerce' ),
    'placeholder' => __( 'House number and street name', 'woocommerce' ),
), $address );

yourtheme/woocommerce/cart/shipping-calculator.php


<form id="custom-login-form">
  <input type="text" name="username" placeholder="Username or Email" required>
  <input type="password" name="password" placeholder="Password" required>
  <button type="submit">Login</button>
  <div id="login-response"></div>
</form>
function.php
function custom_login_scripts() {
    wp_enqueue_script('custom-login-ajax', get_template_directory_uri() . '/js/custom-login.js', ['jquery'], null, true);

    wp_localize_script('custom-login-ajax', 'customLogin', [
        'ajax_url' => admin_url('admin-ajax.php'),
        'nonce'    => wp_create_nonce('custom_login_nonce'),
    ]);
}
add_action('wp_enqueue_scripts', 'custom_login_scripts');



function handle_custom_login_ajax() {
    check_ajax_referer('custom_login_nonce', 'nonce');

    $creds = [];
    $creds['user_login']    = $_POST['username'];
    $creds['user_password'] = $_POST['password'];
    $creds['remember']      = true;

    $user = wp_signon($creds, is_ssl());

    if (is_wp_error($user)) {
        wp_send_json_error(['message' => $user->get_error_message()]);
    } else {
        wp_send_json_success(['message' => 'Login successful!', 'redirect' => home_url()]);
    }
}
add_action('wp_ajax_custom_login', 'handle_custom_login_ajax');
add_action('wp_ajax_nopriv_custom_login', 'handle_custom_login_ajax');


js
jQuery(document).ready(function($) {
  $('#custom-login-form').on('submit', function(e) {
    e.preventDefault();

    $.ajax({
      type: 'POST',
      url: customLogin.ajax_url,
      data: {
        action: 'custom_login',
        username: $('input[name="username"]').val(),
        password: $('input[name="password"]').val(),
        nonce: customLogin.nonce
      },
      success: function(response) {
        if (response.success) {
          $('#login-response').html(response.data.message);
          window.location.href = response.data.redirect;
        } else {
          $('#login-response').html(response.data.message);
        }
      }
    });
  });
});
