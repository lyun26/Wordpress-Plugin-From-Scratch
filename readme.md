# Wordpress Plugin From Scratch

## Init process of plugin
    add_action( 'init', array( $this, 'custom_post_type' ) );

## register style script
    add_action( 'admin_enqueue_scripts', array( $this, 'enqueue' ) ); 
    wp_enqueue_style( 'mypluginstyle', plugins_url( '/assets/mystyle.css', __FILE__ ) );
    wp_enqueue_script( 'mypluginscript', plugins_url( '/assets/myscript.js', __FILE__ ) );

## process when activating
    $alecadddPlugin = new AlecadddPlugin();
    register_activation_hook( __FILE__, array( $alecadddPlugin, 'activate' ) );

## process when deactivating
    require_once plugin_dir_path( __FILE__ ) . 'inc/alecaddd-plugin-deactivate.php';
	register_deactivation_hook( __FILE__, array( 'AlecadddPluginDeactivate', 'deactivate' ) );

##  rewrite the htaccess rules  for type post
    flush_rewrite_rules();   


## custom post type
    register_post_type( 'book', ['public' => true, 'label' => 'Books'] );

## add setting link
    $this->plugin = plugin_basename( __FILE__ );    // base path
    add_filter( "plugin_action_links_$this->plugin", array( $this, 'settings_link' ) );

    public function settings_link( $links ) {
			$settings_link = '<a href="admin.php?page=alecaddd_plugin">Settings</a>';
			array_push( $links, $settings_link );
			return $links;
		}

## Composer NameSpace
    composer init
    Adding below script
        "autoload": {
            "psr-4": {"Inc\\": "./inc"}
        }

    composer init
    composer dump-autoload   

    Add below script into main php file
        if ( file_exists( dirname( __FILE__ ) . '/vendor/autoload.php' ) ) {
            require_once dirname( __FILE__ ) . '/vendor/autoload.php';
        }

## Add Admin Menu
    add_action( 'admin_menu', array( $this, 'add_admin_pages' ) );
    add_menu_page( 'Alecaddd Plugin', 'Alecaddd', 'manage_options', 'alecaddd_plugin', array( $this, 'admin_index' ), 'dashicons-store', 110 );

    public function admin_index() {
		require_once PLUGIN_PATH . 'templates/admin.php';
	}

    // add subpage
    add_submenu_page( $page['parent_slug'], $page['page_title'], $page['menu_title'], $page['capability'], $page['menu_slug'], $page['callback'] );

## Register Admin Custom Fields
    1. register settings  --  encapsulate the fields
    register_setting( $setting["option_group"], $setting["option_name"], ( isset( $setting["callback"] ) ? $setting["callback"] : '' ) );

    2. add settings section -- print settings registered with callback what we are gonna print.
    add_settings_section( $section["id"], $section["title"], ( isset( $section["callback"] ) ? $section["callback"] : '' ), $section["page"] );

    3. add fields
    add_settings_field( $field["id"], $field["title"], ( isset( $field["callback"] ) ? $field["callback"] : '' ), $field["page"], $field["section"], ( isset( $field["args"] ) ? $field["args"] : '' ) );

    Insert the below into page template
    <?php settings_errors(); ?>
	<form method="post" action="options.php">
		<?php 
			settings_fields( 'alecaddd_options_group' );
			do_settings_sections( 'alecaddd_plugin' );
			submit_button();
		?>
	</form>
