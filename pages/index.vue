<template lang="pug">
.rootIndex

	section.storytelling
		.filtroBlur
			img.noMovil(src="/imagenes/portada.webp" alt="Súmate a cambiar Chile")
			img.noCompu(v-if="cargado" src="/imagenes/portadaMovil.webp" alt="Súmate a cambiar Chile")
			img.imgTiny(v-if="!cargado" src="/imagenes/portadaMovilTiny.webp" alt="Súmate a cambiar Chile")


	section.seccionCuna
		.curva.curvaSuperior

		.noCompu.sobreCurva
			.contenido
				div Súmate respondiendo nuestra consulta ciudadana o participando en las mesas ciudadanas, donde podrás compartir tus ideas, anhelos y opiniones para aportar a la construcción de este proyecto transformador.
		.noMovil.sobreCurva
			.contenido
				h1 Mesas ciudadanas
				h2 ¿Cómo participar?
				div Súmate respondiendo nuestra consulta ciudadana o participando en las mesas ciudadanas, donde podrás compartir tus ideas, anhelos y opiniones para aportar a la construcción de este proyecto transformador.

		a.btnparticipa.bold(href='https://tll5o6hb21g.typeform.com/to/gRt8fnSE' target="_blank" rel="noreferer noopener" @click="$gtm.push({ event: 'link-home', hacia: 'encuesta participa'})") Consulta ciudadana
		a.btnparticipa.bold(href='https://participa.boricpresidente.cl/' target="_blank" rel="noreferer noopener" @click="$gtm.push({ event: 'link-home', hacia: 'participa.boricpresidente.cl'})") Participa Aquí

		img.noMovil(src="/imagenes/diversidad.webp" alt="Diversidad"  v-if="cargado").imgSaludo
		img.noCompu(src="/imagenes/diversidadMovil.webp" alt="Diversidad" v-if="cargado").imgSaludo
		img.imgTiny(src="/imagenes/diversidadMovilTiny.webp" alt="Diversidad" v-if="!cargado").imgSaludo

	.precarga(v-if="!cargado")
	.postcarga(v-if="cargado")
		section.seccionParticipa
			.curva.curvaSuperior
			.contenido
				.lado
					h2.titulo #[span.primero Inscríbete]
						div Únete a la campaña
					.texto
						p Las transformaciones que Chile necesita las hacemos 		entre todas y todos.
						P Déjanos tu mail y recibirás más información sobre la 		campaña.

				.lado
					.suscribirse
						participaant
			img.noMovil(src="/imagenes/participa.webp" alt="Participa")
			img.noCompu(src="/imagenes/participaMovil.webp" alt="Participa")


		section.seccionArmaTuCampana
			.curva.curvaSuperior

			mixin linkDescarga
				.kitGrafico
					.texto Descarga tu
					a(href="https://drive.google.com/drive/folders/	1vwqqSnxHIyv9wI617h8pUers1OudaBo0" target="_blank" 	rel="noreferer noopener" @click="$gtm.push({ event: 'link-home', hacia: 'Decarga Kit grafico'})").boton KIT GRÁFICO AQUÍ

			.contenido
				.lado
					h2 Arma tu
					h3 Campaña
					p Descarga nuestro kit y crea tus propios insumos de 	campaña. #[br]¡De ti depende!
					.noMovil
						+linkDescarga


				.lado.ladoImagen
					.cajaHerramienta
						img(src="/imagenes/cajaHerramientas.webp" 	alt="Herramientas")
						.circulo

			.noCompu
				+linkDescarga


		section.seccionaporta

			mixin linkServel
				.linkservel
					a.boton(href="https://aportes.servel.cl/servel-aportes/	inicio.xhtml" target="_blank" rel="noreferer noopener" @click="$gtm.push({ event: 'link-home', hacia: 'Home Aportes Servel'})") 	QUIERO APORTAR
					n-link.instrucciones(to="/aporta" @click.native="tag('instrucciones')") Ver instrucciones para 	donar

			.curva.curvaSuperior

			.contenido
				.lado
					h2 Súmate
					h3 y Aporta
					p El cambio lo financiamos las personas comunes. Aporta y 	construyamos un Chile donde el dinero no haga la diferencia.
					.noMovil
						+linkServel
				.lado.ladoImagen
					Chanchito
				.noCompu
					+linkServel

		section.propuestas
			.ondaonda
				wave.wave
			.zonaCombi
				Combi
			.curva.curvaSuperior

			.curva.curvaSuperior
			.contenido
				.textoseccion
					h2.titulo Propuesta
					h3 Programática
					p Nuestro Gobierno impulsará grandes cambios, paso a paso, 	sin dejar a nadie fuera.
					p ¿Quieres conocer parte de nuestras propuestas?
				.link
					nuxt-link(to="/propuestas" @click.native="tag('Propuestas')").linkpropuestas VER PROPUESTAS

</template>

<script>
import wave from '~/static/svg/wave.svg'
export default {
	components: { wave },
	data () {
		return {
			cargado: null
		}
	},
	head () {
		const titulo = 'Boric Presidente'
		const descripcion = 'Votemos para hacer realidad un Chile en que nadie quede fuera. Votemos Gabriel Boric Presidente'
		const imagen = '/imagenes/portadaMovil.webp'
		const url = 'https://boricpresidente.cl'
		const meta = this.$eo({
			titulo,
			descripcion,
			imagen,
			url
		})
		meta.titleTemplate = '%s'
		return meta
	},
	mounted () {
		this.cargado = true
	},
	methods: {
		tag (valor) {
			console.log('tag')
			this.$gtm.push({ event: 'link-home', hacia: valor })
		}
	}
}
</script>

<style lang="sass" scoped>
@import '~/estilos/utils'
@import '~/estilos/paleta'
@import '~/estilos/animaciones'

section
	position: relative
	// min-height: 50vh
	.contenido
		width: 100%
		max-width: 100%
		padding-left: 1em
		padding-right: 1em
		+compu
			padding-top: 2em
			padding-left: 4em
			padding-right: 4em
	.curvaSuperior
		display: flex
		position: absolute
		top: 0
		left: 0
		right: 0
		width: 100vw
		background-color: black
		justify-content: center
		clip-path: ellipse(65% 100% at 50% 100%)

		height: 3em
		margin-top: -1.2em
		+compu
			height: 5em
			margin-top: -2em
		z-index: 1
		+ *,
		~ .sobreCurva
			position: relative
			z-index: 2

	.ladoImagen
		display: flex
		justify-content: center
		align-items: center

.storytelling
	display: flex
	justify-content: center
	max-height: calc(100vh - 5em)
	img
		width: 119.45%
		height: 100%
		max-width: 100%
		// max-height: 100%
	.imgTiny
		width: 100vw
		// height: 90%
		filter: blur(4px)
	.filtroBlur
		min-height: 250px
		width: 100vw
	+compu
		background-image: url('/imagenes/portada.webp')
		background-size: cover
		background-repeat: no-repeat
		.filtroBlur
			display: flex
			width: 100%
			height: 100%
			backdrop-filter: blur(8px)
			justify-content: center
		img
			margin: 0 0 -2.5em 0
			width: 100%
			height: 53.4%
			max-width: 100vw

.seccionCuna
	position: relative
	background-color: $verde1
	min-height: 20em
	display: flex
	flex-flow: column nowrap
	justify-content: center
	background-color: $verde1
	color: white
	text-align: center
	.curva
		background-color: $verde1
		//+movil
			height: 3em
			margin-top: -1.2em
		//+compu
			margin-top: -2em

	.contenido
		padding: 0 1em
		line-height: 1.3
		width: 400px
		max-width: 100%
		margin: 0 auto
		font-size: 1.2rem

		h1, h2
			font-style: italic
		h1
			font-size: 2.5em
			font-weight: 900
			color: #fff
			line-height: 1.3em
		h2
			color: $verde3
			font-size: 2.5em
			font-weight: 900
			margin-top: -.7em

	.btnparticipa
		display: block
		margin: 0 auto
		cursor: pointer
		text-transform: uppercase
		background-color: $verde3
		color: $verde1
		padding: .5em 1.3em
		border-radius: 4px
		margin-top: .5em
		z-index: 5
	.imgSaludo
		width: 100%
		height: 66.55%
		min-height: 10em
		// width: 100vw
		z-index: 0
		margin-top: -9vw

	+compu
		.contenido
			width: 600px
			font-size: 1.2rem
			h1
				font-size: 2.8em
		.btnparticipa
			font-size: 1.1rem
			margin-top: 1em

		.imgSaludo
			margin-top: -6vw


.precarga
	height: 100vh
	width: 100vw
	background-color: $verde2
.seccionParticipa
	position: relative
	background-color: $verde2
	color: white
	.curva
		background-color: $verde2
	.contenido
		position: relative
		z-index: 3
		text-align: center
		.titulo
			max-width: 100%
			line-height: 1.2
			font-size: 2.3rem
			color: #fff
			font-weight: 900
			.primero
				font-size: 3rem
				font-weight: 400
				font-style: italic
				color: $verde3

		.texto
			font-style: italic
			font-size: 1.2rem
		.suscribirse
			padding: 0 .5em
			display: flex
			justify-content: center
	img
		width: 100%
		height: 51.82%
	+compu
		.contenido
			display: flex
			flex-flow: row nowrap
			text-align: left
			justify-content: space-around
			.lado
				flex: 350px 0 1



.seccionArmaTuCampana
	background-color: $petroleo2
	display: flex
	flex-flow: column
	align-items: center
	position: relative
	justify-content: center
	padding-bottom: 5.5em
	.curva
		background-color: $petroleo2
	.contenido
		text-align: center

		.texto
			display: flex
			flex-flow: column
			// padding: 0 1em
		h2,
		h3
			font-size: 3rem
			line-height: .7em
		h2
			color: $verde3
			font-style: italic
		h3
			font-weight: 900
			color: #fff
		p
			color: #fff
			font-style: italic
			font-size: 1.2rem
			// padding: 0 .5em

	.cajaHerramienta
		$lado: 11em
		width: $lado
		height: $lado
		position: relative
		img
			position: relative
			width: 100.13%
			height: 100%
			z-index: 1
			padding: 1em 0 1em 0
		.circulo
			position: absolute
			top: 50%
			left: 50%
			right: 0
			bottom: 0
			width: $lado
			height: $lado
			background: $petroleo3
			border-radius: 50%
			transform: translate(-25%, -50%) scale(.9)
		+compu
			$lado: 18em
			width: $lado
			height: $lado
			.circulo
				width: $lado
				height: $lado
			.circulo
				transform: translate(-30%, -50%) scale(.9)

	.kitGrafico
		display: flex
		flex-flow: column
		align-items: center
		text-align: center
		color: $azul2
		font-weight: 700
		.texto
			font-size: 1em
			font-style: italic
		.boton
			background-color: $verde3
			color: $azul2
			font-size: 1.1rem
			padding: .5em 1.5em
			&:visited
				color: $azul2
	+compu
		.contenido
			display: flex
			flex-flow: row nowrap
			justify-content: space-around
			text-align: left
			padding-top: 2em
			.lado
				flex: 250px 0 1
				&.ladoImagen
					order: -1
			.kitGrafico
				display: block
				.texto
					text-align: left



section.seccionaporta
	background-color: $azul2
	position: relative
	.curva
		background-color: $azul2

	.linkservel
		display: flex
		flex-flow: column nowrap
		align-items: center
		justify-content: center
		padding: .5em 0
		.boton
			color: $azul2
			background-color: $verde3
			font-size: 1.1rem
			padding: .4em 1.5em
		.instrucciones
			color: $verde3
			font-style: italic
			padding: .5em 0
		+compu
			align-items: unset
			justify-content: unset
			.boton
				margin: 0 auto 0 0

	.contenido
		display: flex
		flex-flow: column nowrap
		justify-content: center
		align-items: center
		text-align: center
		padding-bottom: 6em
		h2,
		h3
			font-size: 3rem
			margin: 0
			line-height: 1
			font-style: italic
		h2
			color: $verde3
		h3
			font-weight: 900
			color: #fff
		p
			margin: 1em 0
			font-style: italic
			font-size: 1.2rem

	+compu
		.contenido
			display: flex
			flex-flow: row nowrap
			text-align: left
			justify-content: space-around
			padding-bottom: 14em
			.lado
				flex: 350px 0 1

section.propuestas
	position: relative
	background-color: rgba(249, 58, 121, 1)
	padding-bottom: 4em
	.curva
		background-color: rgba(249, 58, 121, 1)
	.ondaonda
		position: absolute
		background-color: #268CAF
		width: 100%
		height: 0
		bottom: 100%
		.wave
			display: flex
			position: absolute
			width: 100vw
			right: 0
			left: 0
			bottom: 100%
		+compu
			height: 2em
			.wave
				height: auto

	.zonaCombi
		position: relative
		z-index: 3

	.contenido
		display: flex row nowrap
		text-align: center
		justify-content: center
		max-width: 100%
		heigth: 10em
		background-color: rgba(249, 58, 121, 1)
		+movil
			padding-top: 1em
		+compu
			padding-bottom: 1em
		.textoseccion
			h2,
			h3
				font-size: 3rem
				line-height: 1.5rem
				font-weight: 900
				font-style: italic
				color: $verde3
			h2
				font-weight: 400
				color: #fff
			p
				color: #fff
				font-style: italic
				font-size: 1.2rem
				&:nth-of-type(1)
					padding-top: .5em

		.link
			padding: 1em 0
			display: flex
			justify-content: center
			.linkpropuestas
				font-size: 1.1rem
				display: flex
				padding-top: .2em
				justify-content: center
				border-radius: 5px
				align-items: center
				width: 250px
				max-width: 85%
				height: 2em
				font-weight: 900
				color: $azul2
				background-color: $verde3
	+compu
		.contenido
			margin: 0 auto
</style>


