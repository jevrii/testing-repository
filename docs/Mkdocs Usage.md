## Some mkdocs command for building the site

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

	docs/
		assets/			# Store site logos.
		Stylesheets/	# Store additional css settings.
			color.css	# Set the theme color of the site
		index.md	# The default homepage.
		about.md	# Some mkdocs guide.
		...			# Tutorial materials, other markdown pages, images and other files.
		
	mkdocs.yml		# The configuration file, all settings are here.
	README.md		# Description of the site repository.
