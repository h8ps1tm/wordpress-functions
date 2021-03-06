# Useful Functions to use in any instalation of WordPress

Useful functions to have on your WordPress website:

## Change Media Attributes on Upload

```
function changeOnImageUpload($post_ID) {

    // Check if uploaded file is an image, else do nothing
    if (wp_attachment_is_image($post_ID))
    {
        $title = get_post($post_ID)->post_title;
        // Sanitize the title: remove hyphens, underscores & extra
        // spaces:
        $title = preg_replace( '%\s*[-_\s]+\s*%', ' ', $title);

        // Create an array with the image meta (Title, Caption,
        // Description) to be updated
        $image_meta = array(
            'ID' => $post_ID,
            'post_title' => $title,
            'post_excerpt' => $title,
            'post_content' => $title,
        );

        // Set the image Alt-Text
        update_post_meta($post_ID, '_wp_attachment_image_alt', $title);
        // Set the image meta (e.g. Title, Excerpt, Content)
        wp_update_post($image_meta);
    }
}
```

## Load Image on correct format

```
function loadImage( $post_id, $size ) {
    $image_id    = get_post_thumbnail_id( $post_id );
    return wp_get_attachment_image( $image_id, $size );
}
```

## Support SVG on Media Upload

```
function activateSVG( $file_types ) {
    $new_filetypes = array();
    $new_filetypes['svg'] = 'image/svg+xml';
    $file_types = array_merge($file_types, $new_filetypes );
    return $file_types;
}
add_filter('upload_mimes', 'add_file_types_to_uploads');
