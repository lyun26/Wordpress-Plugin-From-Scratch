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