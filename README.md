Puffer Fish CSS
===============

This is a simple and clean bootstrap framework to help you kickstart your web application in minutes.

It features a simple and intuitive semantic grid system, designed for both fixed and fluid layouts. It offers support for column substitution (more on that below) and responsive designs, all to help you create a basic layout in no time.
A collection of components come in handy when you need to add buttons and forms.
And, because it's a LESS framework, all of this is combined and compiled into one single `.css` file.


File structure
--------------

The framework is divided into logical parts separated by scope:

	/css
		/grid
			system.less
			structure-large.less
			structure-normal.less
			structure-netbook.less
			structure-tablet.less
			structure-mobile.less
		/styles
			/buttons
				generic-buttons.less
				super-awesome.less
			/forms
				default.less
			/typography
				google-webfonts.less
				styles.less
			/layout
				default.less
		/utils
			/reset
				yui.less
                h5bp-normalize.less
                h5bp-main.less
                vanilla-css.less
			/variables
				colors.less
				dimensions.less
				images.less
			/mixins
				common.less
				helpers.less
		merged.less
	/js
		/ie
			html5.js
			ie9.js
		/jquery
	index.html

Too much? You'll see how very simple it actually is. Let's get into it!


The grid system
---------------

Thanks to LESS and inspired by [semantic.gs](http://semantic.gs/), the grid engine lets you draw your grid layout without adding `.spanX` or `.grid_Y` classes to your markup.
It comes with mixins to easily define sections, wrappers, columns and blocks of content directly inside your LESS declarations.

# Basics

	<div id="wrapper">
		<header>
			...
		</header>
		<article>
			<h1>...</h1>
			<p>...</p>
		</article>
		<aside>
			<div id="archive">...</div>
			<div class="banner">...</div>
		</aside>
		<footer>
			...
		</footer>
	</div>

... and the LESS declarations

	@gridLayoutWidth:	960px;
	@gridGutter:		20px;
	@gridColumns:		12;

	#wrapper {
		.gridWrapper();
	}
	header {
		.gridColumn(12);
	}
	article {
		.gridColumn(9);

		h1, p {
			.gridBox(9);
		}
	}
	aside {
		.gridColumn(3, right);

		#archive, .banner {
			.gridBox(3);
		}
	}
	footer {
		.gridColumn(12);
	}

Clean markup and intuitive declarations.

# Column substitution

Sometimes, a design needs to break from the overall layout. For example, we have a 9 columns `<article>` on the left and a 3 columns `<aside>` floated to the right. But inside the `<article>` we have a section that need to be separated in half.
Of course, on the standard grid system we cannot do that, unless we change the grid to 24 columns, with a much smaller column size. But that means we have to rewrite all the other declarations we have so far. Which is dumb!
The column substitution engine does exactly that. And it's very easy too. We just have to define a normal `.gridBox()` (that spans across grid columns), but with a different width ratio.

	<article>
		<h1>...</h1>
		<p>...</p>

		<div id="comments">...</div>
		<div id="related">...</div>
	</article>

And the grid declarations:

	article {
		.gridColumn(9);

		h1, p {
			.gridBox(9);
		}

		#comments, #related {
			.gridBox(1, @columns, left, 2/9);
		}
	}

Here, the ratio is 2/9, meaning that for the `#comments` and `#related` we transform the 9 columns of the <article> into just 2. Each of these two boxes span across one of this new columns.
The `@columns` parameter is a reference number used in the calculations of the new columns width.
For fixed layouts, this is always the number of columns of the full grid (here 12), whereas for fluid layouts (because of nested percentages) we have to send the number of columns of the containing element. Like so:

	article {
		@columns: 9

		.gridColumn(@columns);

		h1, p {
			.gridBox(@columns);
		}

		#comments, #related {
			.gridBox(1, @columns, left, 2/9);
		}
	}

