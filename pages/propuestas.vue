<template lang="pug">
.propuestasRoot

	n-child(@montado="verSiHayQueAbrirUnaPropuesta")
	//- .zonaImagen(v-if="imagen")
	//- 	img.imagenPrograma(:src='imagen.url' :alt='altImg')
	.encabezado
		h1 Propuesta
		h1 Programática
		h1 para un nuevo Chile
		p Nuestro Gobierno impulsará grandes cambios, paso a paso, sin dejar a nadie fuera.
		p ¿Quieres conocer parte de nuestras propuestas?

	//- section.pilares(v-if="pilares")
	//- 	.contenido
	//- 		.ql-editor.contenidoHTML(v-html='pilares')


	section.propuestas(v-if="propuestas")

		.caja-propuestas

			.propuesta(v-for='propuesta in propuestas' :key='propuesta.id' @click='abrirPropuesta(propuesta.id, propuesta.Slug)')
				.prop
					img.imagenDePropuesta(:src='propuesta.imagen.url' :alt="propuesta.textoAlternativoImagen")
					h2.tituloPropuesta {{ propuesta.titulo }}


			a-modal.modalPropuesta(:visible="mostrandoPropuesta" :footer="null" @close="mostrandoPropuesta = false" @cancel="mostrandoPropuesta = false" centered :width="null")
				div(slot="title")
					.pretitulo Propuestas programáticas
					h2.titulo {{propuestaMostrada && propuestaMostrada.titulo}}
					//- .modoVisualizacion(v-if="propuestaMostrada && propuestaMostrada.pdfURL")
					//- 	.modo(@click="modoVisualizacion = 'html'" :class="{activo: modoVisualizacion === 'html'}") Ver texto
						//- .modo(@click="modoVisualizacion = 'pdf'" :class="{activo: modoVisualizacion === 'pdf'}") Ver en PDF
						//- a.modo(:href="propuestaMostrada.pdfURL" download target="_blank" rel="noreferer noopener") Descargar PDF
				.cuerpoPropuesta(v-if="propuestaMostrada")
					transition-group(mode="out-in")
						.pdf(v-show="modoVisualizacion === 'pdf'" key="pdf")
							iframe(:src='`https://docs.google.com/viewer?url=${propuestaMostrada.pdfURL}&embedded=true`' frameborder='0' height='500px' width='100%')
						.html(v-show="modoVisualizacion === 'html'" key="html")
							.ql-editor.contenidoHTML(v-html='propuestaMostrada.html')

	.zonaDescargas
		a.descargable(v-if="programaArchivo" :href='programaArchivo.url' target="_blank" rel="noreferer noopener" download :name="programaArchivo.name" :title="programaArchivo.name")
			.dentro
				.oicono.descargar
				.texto Descarga nuestro programa (en construcción)
	.textoCierre
		p Estas y muchas otras ideas se están pensando en nuestras mesas de participación.
		p Súmate y decidamos las propuestas de nuestro programa.
		a.btnparticipa.bold(href='https://participa.boricpresidente.cl/' target="_blank" rel="noreferer noopener" @click="$gtm.push({ event: 'link-home', hacia: 'Propuestas participa.boricpresidente.cl'})") Participa Aquí



</template>

<script>
export default {
	scrollToTop: true,
	async asyncData ({ app }) {
		// console.log('context keys', Object.keys(context))
		const _ = app.$lodash
		// console.log('asyncData')
		const respuesta = await app.$olicitar(`${process.env.cmsURL}/programa`)
		const propuestas = respuesta.propuestas // Array

		const data = {
			propuestas: [],

			seo: null,

			imagen: null,
			altImg: null,
			// pilares: null,
			programaArchivo: null,


			propuestaIdMostrada: null,
			mostrandoPropuesta: null,
			modoVisualizacion: 'html'
		}
		data.propuestas = _.map(propuestas, p => {
			p.contenido = app.$sanitizar(p.contenido)
			return p
		})

		// SEO
		data.seo = respuesta.SEO
		// IMAGEN
		const componenteImagen = respuesta.componenteImagen
		data.imagen = componenteImagen.imagen
		data.altImg = componenteImagen.textoAlternativoImagen
		// PILARES.
		// data.pilares = app.$sanitizar(respuesta.Texto_pilares)
		// PROGRAMA COMPLETO
		data.programaArchivo = _.get(respuesta, ['Archivo_programa'])

		return data
	},
	data () {
		return {
			propuestas: [],

			seo: null,

			imagen: null,
			altImg: null,
			// pilares: null,
			programaArchivo: null,


			propuestaIdMostrada: null,
			mostrandoPropuesta: null,
			modoVisualizacion: 'html'
		}
	},
	// solicita info a cms
	head () {
		// if (!this.seo) return {}
		const titulo = this._.get(this.seo, ['titulo_pag'], 'Propuesta Programática')
		const descripcion = this._.get(this.seo, ['descripcion_pag'], 'Nuestro Gobierno impulsará grandes cambios, paso a paso, sin dejar a nadie fuera. ¿Quieres conocer parte de nuestras propuestas?')
		const imagen = '/imagenes/portadaMovil.web'
		const url = 'https://boricpresidente.cl/propuestas'
		const obj = this.$eo({
			titulo,
			descripcion,
			imagen,
			url
		})
		obj.link = obj.link || []
		obj.link.push({ hid: 'quill', rel: 'stylesheet', href: 'https://cdn.quilljs.com/1.0.0/quill.snow.css' })
		return obj
	},

	computed: {
		propuestaMostrada () {
			if (!this.propuestaIdMostrada) return null
			const propuestaBruta = this._.find(this.propuestas, p => p.id === this.propuestaIdMostrada)
			if (!propuestaBruta) return null

			return {
				titulo: propuestaBruta.titulo,
				html: propuestaBruta.contenido,
				pdfURL: this._.get(propuestaBruta, ['archivoPDF', 'url'])
			}
		}
	},
	watch: {
		mostrandoPropuesta (v) {
			if (!v) {
				if (this.propuestaMostrada) this.$router.push('/propuestas')
				setTimeout(() => {
					this.propuestaIdMostrada = false
					this.modoVisualizacion = 'html'
				}, 400)
			}
		},
		propuestaIdMostrada (v) {
			if (v) this.mostrandoPropuesta = true
		}
	},
	mounted () {
		window.propuesta = this
	},
	methods: {
		abrirPropuesta (propuestaID, slug) {
			this.propuestaIdMostrada = propuestaID
			this.$nextTick(() => {
				if (this.propuestaMostrada) this.$router.push(`/propuestas/${slug}`)
			})
		},
		verSiHayQueAbrirUnaPropuesta ({ propuestaSlug }) {
			const propuestas = this.propuestas
			const propuestaParaAbrir = this._.filter(propuestas, ['Slug', propuestaSlug])
			console.log('prop a abrir', propuestaParaAbrir, propuestas)
			if (propuestaSlug) {
				const propuestaID = propuestaParaAbrir[0]._id
				this.abrirPropuesta(propuestaID, propuestaSlug)
			}
		}
	}
}



</script>

<style lang="sass" scoped>
@import '~/estilos/utils'
@import '~/estilos/paleta'
.propuestasRoot
	background-color: $azul2
	section
		padding: 2em
		+movil
			padding: 0

// ENCABEZADO
.encabezado
	text-align: center
	padding-top: 1.5rem
	h1
		padding: 0 1rem 0 1rem
		margin: 0
		line-height: 1
		color: $verde3
		font-style: italic
		font-weight: 900
		font-size: 3em
		&:nth-child(1)
			padding-top: 2rem
		&:nth-child(3)
			color: #fff
			font-weight: 400
	p
		padding: 0 2.5rem
		color: #fff
		font-size: 1.2rem
		&:nth-of-type(1)
			padding-top: 4rem
	+movil
		p
			padding: 0 1em


// CONTENIDO HTML
.contenidoHTML
	width: 900px
	max-width: 100%
	margin: 0 auto
	// border: 3px dashed orangered
	background-color: #fff
	// box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px
	padding: 4rem
	+movil
		padding: 1rem
		font-size: .889em
	text-align: justify
	justify-content: justify
	::v-deep
		*
			font-family: lexia, Lucida Sans, Lucida Grande, sans-serif
			line-height: 1.4
		h1
			font-weight: 900
			font-style: italic
			margin-top: 2rem
			color: #19CBB5
		h2
			font-size: 1.4rem
			margin-bottom: 1em
			margin-top: 1.4rem
			font-weight: 900
			font-style: italic
			color: #094C67
		h1 + p
			margin-top: 2rem
		p,
		li
			font-size: 1rem
			line-height: 1.8
			margin-bottom: .8rem
			+movil
				line-height: 1.6
		strong
			font-weight: bold
			font-style: italic
		b
			font-weight: bold
		ol
			margin: 2em 0
		a
			all: revert

.zonaImagen
	.imagenPrograma
		min-width: 100%
		min-height: 230px
		position: relative
		left: 50%
		transform: translateX(-50%)

.zonaDescargas
	display: none
	// display: flex
	justify-content: center
	padding: 1em 1em 6em 1em
	.descargable
		padding-bottom: 3em
		.dentro
			display: flex
			align-items: center
			background-color: transparentize($colorBody, .9)
			.oicono
				color: $verde3
				flex: auto 0 0
				font-size: 3em
				margin: 2rem
			.texto
				color: #fff
				flex: auto 1 1
				margin: 2rem 2rem 2rem 0

.textoCierre
	display: flex
	flex-flow: column
	justify-content: center
	padding: 1em 1em 6em 1em
	align-items: center
	font-size: 1.2rem
	text-align: center
	p
		&:nth-child(1)
			margin-bottom: .3em
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
// PILARES
// .pilares
// 	+movil
// 		.contenidoHTML
// 			padding: 5em 1em
// PROPUESTAS
.propuestas
	z-index: 0
	cursor: pointer
	+movil
		.caja-propuestas
			padding: 3.5em 0

	.caja-propuestas
		display: flex
		flex-flow: row wrap
		justify-content: center
		.propuesta
			// flex: 12em 0 0
			margin: 10px
			text-align: center
			padding: 5px
			width: 250px
			height: 250px
			background-color: rgba(14, 107, 139, 1)
			.prop
				display: flex
				flex-flow: column
				align-items: center
				justify-content: center
				.imagenDePropuesta
					$lado: 130px
					padding-top: 1em
					max-width: $lado
					max-height: $lado
					z-index: 1
				.tituloPropuesta
					margin-top: 1rem
					font-size: 1rem
					padding: 0 .3em
					font-style: italic
					color: #fff

				+movil
					width: 250px
					height: 250px
					.imagenDePropuesta
						$lado: 100px
						max-width: $lado
						max-height: $lado


		.contenido-propuesta
			width: 80vw
			height: 40vh
			overflow-y: scroll
			transition: .5s
			top: 50%
			background: rgb(255, 255, 255)
			padding: 20px
			+salir
				max-height: 0
				opacity: 0
			+saliendo
				max-height: 100vh
				overflow: hidden


.modalPropuesta
	::v-deep
		.ant-modal-mask
			backdrop-filter: blur(.5em)
		.ant-modal
			width: auto
			max-width: 100%
			margin: 0
			// color: inherit
			.ant-modal-content
				height: 80vh
				background-color: transparent
				+movil
					height: 100vh
				// background-color: red
				display: flex
				flex-flow: column nowrap
				.ant-modal-header
					flex: auto 0 0
					padding: 2em
					background-color: transparentize($fondoBody, .2)
					backdrop-filter: blur(1em)
					width: 900px
					background-color: #19CBB5
					.pretitulo
						color: rgba(219, 248, 123, 1)
						font-weight: 900
						margin-bottom: 1em
						opacity: .6
					.titulo
						color: #fff
						font-weight: 900
						font-style: italic
						line-height: 1.4
						display: flex
						max-width: 95vw
					.modoVisualizacion
						color: rgba(23, 117, 150, 1)
						display: flex
						margin-top: .5em

						.modo
							cursor: pointer
							font-size: .8em
							opacity: .8
							transition: opacity .15s ease
							&.activo
								// opacity: 1
								color: lighten(rgba(23, 117, 150, 1),10%)
						.modo + .modo
							margin-left: 1rem
					+movil
						padding: 1em
						.pretitulo
							font-size: .8em
						.titulo
							font-size: 1em
				.ant-modal-body
					flex: auto 1 1
					max-height: 100%
					overflow-y: auto
					padding: 0
					display: flex
					flex-flow: column nowrap
					width: 900px
					max-width: 100%
					.cuerpoPropuesta
						&,
						& > span,
						& > span > div,
						& > span > div > iframe
							flex: auto 1 1
							display: flex
							flex-flow: column nowrap
					.pdf,
					.html
						flex: auto 1 1
						width: 900px
						max-width: 100%
					+salir
						opacity: 0

</style>

