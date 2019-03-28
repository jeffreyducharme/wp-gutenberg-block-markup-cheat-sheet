# wp-gutenberg-block-wrapper-codes-cheat-sheet
The comment codes gutenberg actually wraps blocks with. Here are all the common blocks and some php search and replace code for helping you batch convert html to block style html

#blocks

<!-- wp:paragraph -->
<p>paragraph text</p>
<!-- /wp:paragraph -->

<div.*?>(.*?)</div>
<!-- wp:paragraph --><div>$1</div><!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Heading Text</h2>
<!-- /wp:heading -->

<!-- wp:heading {\\"level\\":3} --><h3>
<h3>Heading Text</h3>
<!-- /wp:heading -->

<!-- wp:image -->
<figure class="wp-block-image">
	<img src="https://itre.ncsu.edu/wp-content/uploads/2016/08/image002.png" alt=""/>
</figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul>
	<li>list item 1</li>
	<li>list item 2</li>
</ul>
<!-- /wp:list -->

<!-- wp:columns -->
<div class="wp-block-columns has-2-columns">
	<!-- wp:column -->
	<div class="wp-block-column">
		<!-- wp:paragraph -->
			<p>af</p>
		<!-- /wp:paragraph -->
	</div>
	<!-- /wp:column -->

	<!-- wp:column -->
	<div class="wp-block-column">
		<!-- wp:paragraph -->
			<p>some paragraph text in col 2</p>
		<!-- /wp:paragraph -->
	</div>
	<!-- /wp:column -->
</div>
<!-- /wp:columns -->

<!-- wp:media-text {"mediaId":4800,"mediaType":"image"} -->
<div class="wp-block-media-text alignwide">
	<figure class="wp-block-media-text__media">
		<img src="https://somedomain.com/some-image-file-1.png" alt="Featured Image for this post" class="wp-image-4800"/>
	</figure>
	<div class="wp-block-media-text__content">
		<!-- wp:paragraph {"placeholder":"Content…","fontSize":"large"} -->
			<p class="has-large-font-size">some text on the side</p>
		<!-- /wp:paragraph -->
	</div>
</div>
<!-- /wp:media-text -->

<!-- wp:html -->
<section><div><p>some text</p></div></section>
<!-- /wp:html -->

<!-- wp:cover {"url":"https://somedomain.com/some-image-file-1.png","id":4795} -->
<div class="wp-block-cover has-background-dim" style="background-image:url(https://somedomain.com/some-image-file-1.png)"></div>
<!-- /wp:cover -->

<!-- wp:gallery {"ids":[4795,4793,4791]} -->
<ul class="wp-block-gallery columns-3 is-cropped">
	<li class="blocks-gallery-item">
		<figure>
			<img src="https://somedomain.com/some-image-file-1.png" alt="" data-id="4795" data-link="https://somedomain.com/some-image-file-1/" class="wp-image-4795"/>
		</figure>
	</li>
	<li class="blocks-gallery-item">
		<figure>
			<img src="https://somedomain.com/some-image-file-2.png" alt="" data-id="4793" data-link="https://somedomain.com/some-image-file-2/" class="wp-image-4793"/>
		</figure>
	</li>
	<li class="blocks-gallery-item">
		<figure>
			<img src="https://somedomain.com/some-image-file-3.png" alt="" data-id="4791" data-link="https://somedomain.com/some-image-file-3/" class="wp-image-4791"/>
		</figure>
	</li>
</ul>
<!-- /wp:gallery -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>Quote main text</p><cite>quote cite</cite></blockquote>
<!-- /wp:quote -->

<!-- wp:audio {"id":4809} -->
<figure class="wp-block-audio"><audio controls src="https://somedomain.com/some-audio.wav"></audio><figcaption>caption text for audio file</figcaption></figure>
<!-- /wp:audio -->

<!-- wp:file {"id":4810,"href":"https://somedomain.com/test.txt"} -->
<div class="wp-block-file"><a href="https://somedomain.com/test.txt">test</a><a href="https://somedomain.com/test.txt" class="wp-block-file__button" download>Download link text</a></div>
<!-- /wp:file -->

<!-- wp:core-embed/youtube {"url":"https://www.youtube.com/watch?v=WVZ6xPF-ozI","type":"video","providerNameSlug":"youtube","className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio">
	<div class="wp-block-embed__wrapper">
		https://www.youtube.com/watch?v=WVZ6xPF-ozI
	</div>
	<figcaption>caption for embed youtube</figcaption>
</figure>
<!-- /wp:core-embed/youtube -->

<!-- wp:video -->
<figure class="wp-block-video"></figure>
<!-- /wp:video -->

#conversion code 
```
function replace_body_content( $input ) {
	$trans = array(
		"/<p(.*?)>(.*?)<\/p>/" => "<!-- wp:paragraph --><p$1>$2</p><!-- /wp:paragraph -->",
		"/<div(.*?)>(.*?)<\/div>/" => "<!-- wp:paragraph --><p><div$1>$2</div></p><!-- /wp:paragraph -->",
		"/<h([1-6])(.*?)>(.*?)<\/h([1-6])>/" => "<!-- wp:heading {\"level\":$1} --><h$1$2>$3</h$1><!-- /wp:heading -->",
		"/<ul(.*?)>(.*?)<\/ul>/" => "<!-- wp:list --><ul$1>$2</ul><!-- /wp:list -->",
		"/<iframe.*? src=\".*?youtube.com\/embed\/(.*)\?.*?<\/iframe>/" => "<!-- wp:core-embed/youtube {\"url\":\"https://www.youtube.com/watch?v=$1\",\"type\":\"video\",\"providerNameSlug\":\"youtube\",\"className\":\"wp-embed-aspect-16-9 wp-has-aspect-ratio\"} --><figure class=\"wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio\"><div class=\"wp-block-embed__wrapper\">https://www.youtube.com/watch?v=$1</div><figcaption>embed youtube</figcaption></figure><!-- /wp:core-embed/youtube -->",

	);
	$result = preg_replace(array_keys($trans), array_values($trans), $input);
	return $result;
}

$string = "<div class='someclass'><p>sometext</p></div>";

echo replace_body_content( $string );

//returns= <!-- wp:paragraph --><div class='someclass'><!-- wp:paragraph --><p>sometext</p><!-- wp:paragraph --></div><!-- wp:paragraph -->
```

