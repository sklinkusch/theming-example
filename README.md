# theming-example

## Define SCSS variables and link them to CSS variables

```scss
$--link-color: --link-color;
$--text-color: --text-color;
$--page-color: --page-color;
```

## Define theme maps

```scss
$darkmap: (
	$--link-color: #61dafb,
	$--text-color: #ffffff,
	$--page-color: #282c34,
);

$lightmap: (
	$--page-color: #d9d9d9,
	$--text-color: #131313,
	$--link-color: #3322dd,
);
```

## Define a mixin to put the variables

```scss
@mixin spread-map($colormap: ()) {
	@each $key, $value in $colormap {
		#{$key}: $value;
	}
}
```

## Use this mixin to define the CSS variables

```scss
.App-header {
	&.light-theme {
		@include spread-map($lightmap);
	}
	&.dark-theme {
		@include spread-map($darkmap);
	}
}
```

## Make a transition for changing the colors

```scss
.App-header,
.App-header * {
	transition: background-color 1000ms !important;
	transition-delay: 0s !important;
}
```

## Make a function that can be used in components

```scss
@function theme-var($key, $fallback: null, $map: $darkmap) {
	@if not map-has-key($map, $key) {
		@error "key: '#{$key}', is not a key in map: #{$map}";
	}
	@if ($fallback) {
		@return var($key, $fallback);
	} @else {
		@return var($key);
	}
}
```

## Use this functions in components

```scss
@import "./variables";

.App {
	text-align: center;
}

.App-logo {
	height: 40vmin;
	pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
	.App-logo {
		animation: App-logo-spin infinite 20s linear;
	}
}

.App-header {
	background-color: theme-var($--page-color);
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	font-size: calc(10px + 2vmin);
	color: theme-var($--text-color);
}

.App-link {
	color: theme-var($--link-color);
}

@keyframes App-logo-spin {
	from {
		transform: rotate(0deg);
	}
	to {
		transform: rotate(360deg);
	}
}
```
