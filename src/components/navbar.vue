<script>
  import f7NavLeft from './nav-left.vue';
  import f7NavTitle from './nav-title.vue';

  export default {
    components: {
      f7NavLeft,
      f7NavTitle,
    },
    render(c) {
      const self = this;
      let innerEl;
      let leftEl;
      let titleEl;
      if (self.inner) {
        if (self.backLink) {
          leftEl = c('f7-nav-left', {
            props: {
              backLink: self.backLink,
              backLinkUrl: self.backLinkUrl,
            },
            on: {
              'back-click': self.onBackClick,
            },
          });
        }
        if (self.title) {
          titleEl = c('f7-nav-title', {
            props: {
              title: self.title,
            },
          });
        }
        innerEl = c('div', { staticClass: 'navbar-inner', class: { sliding: self.sliding } }, [leftEl, titleEl, self.$slots.default]);
      }
      return c('div', {
        staticClass: 'navbar',
        class: self.classes,
      }, [self.$slots['before-inner'], innerEl, self.$slots['after-inner']]);
    },
    updated() {
      const self = this;
      if (!self.$f7) return;
      self.$nextTick(() => {
        self.$f7.navbar.size(self.$el);
      });
    },
    props: {
      backLink: [Boolean, String],
      backLinkUrl: String,
      sliding: {
        type: Boolean,
        default: true,
      },
      title: String,
      colorTheme: String,
      color: String,
      hidden: Boolean,
      noShadow: Boolean,
      inner: {
        type: Boolean,
        default: true,
      },
    },
    computed: {
      classes() {
        const self = this;
        const co = {
          'navbar-hidden': self.hidden,
        };
        if (self.colorTheme) co[`color-theme-${self.colorTheme}`] = true;
        // if (this.layout) co[`layout-${this.layout}`] = true;
        if (self.noShadow) co['no-shadow'] = true;
        return co;
      },
    },
    methods: {
      hide(animate) {
        const self = this;
        if (!self.$f7) return;
        self.$f7.navbar.hide(self.$el, animate);
      },
      show(animate) {
        const self = this;
        if (!self.$f7) return;
        self.$f7.navbar.show(self.$el, animate);
      },
      size() {
        const self = this;
        if (!self.$f7) return;
        self.$f7.navbar.size(self.$el);
      },
      onBackClick(e) {
        this.$emit('back-click', e);
        this.$emit('click:back', e);
      },
    },
  };
</script>
