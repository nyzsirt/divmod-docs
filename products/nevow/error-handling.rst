==============
Error Handling
==============

Just an example for now.

.. code-block:: python

    from zope.interface import implements

    from twisted.application import service, strports
    from twisted.web import http

    from nevow import appserver, context, inevow, loaders, rend, static, tags as T, url

    # Inspired by http://divmod.org/users/wiki.twistd/nevow/moin.cgi/Custom404Page

    class ErrorPage(rend.Page):
        def render_lastLink(self, ctx, data):
            referer = inevow.IRequest(ctx).getHeader('referer')
            if referer:
                return T.p[T.a(href=referer)['This link'],
                           ' will return you to the last page you were on.']
            else:
                return T.p['You seem to be hopelessly lost.']

    class The500Page(ErrorPage):
        implements(inevow.ICanHandleException)
        docFactory = loaders.xmlstr('''
    <!DOCTYPE html PUBLIC '-//W3C//DTD XHTML 1.0 Strict//EN'
               'http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd'>
    <html xmlns:n='http://nevow.com/ns/nevow/0.1'>
        <head>
            <title>500 error</title>
            <style type='text/css'>
            body { border: 6px solid red; padding: 1em; }
            </style>
        </head>
        <body>
            <h1>Ouchie. Server error.</h1>
            <p>The server is down.</p>
            <p><em>Ohhh. The server.</em></p>
            <p n:render='lastLink' />
        </body>
    </html>''')

        def renderHTTP_exception(self, ctx, failure):
            request = inevow.IRequest(ctx)
            request.setResponseCode(http.INTERNAL_SERVER_ERROR)
            res = self.renderHTTP(ctx)
            request.finishRequest( False )
            return res

    class The404Page(ErrorPage):
        implements(inevow.ICanHandleNotFound)
        docFactory = loaders.xmlstr('''
    <!DOCTYPE html PUBLIC '-//W3C//DTD XHTML 1.0 Strict//EN'
               'http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd'>
    <html xmlns:n='http://nevow.com/ns/nevow/0.1'>
        <head>
            <title>404'd !!!</title>
            <style type='text/css'>
            body { border: 6px solid orange; padding: 1em;}
            </style>
        </head>
        <body>
            <h1>OW! my browser!</h1>
            <p>Were you just making up names of files or what?</p>
            <p>(apologies to http://homestarrunner.com)</p>
            <p n:render='lastLink' />
        </body>
    </html>''')
        def renderHTTP_notFound(self, ctx):
            return self.renderHTTP(ctx)

    class OopsPage(rend.Page):
        def exception(self, ctx, data):
            1/0 # cause an exception on purpose
        docFactory = loaders.stan(exception)

    class RootPage(rend.Page):
        docFactory = loaders.stan(
                T.html[T.head[T.title['Error Pages']],
                       T.body[T.h1['The Only Real Page'],
                              T.a(href='notherebaby')[
                                'This link goes nowhere, pleasantly.'],
                              T.br,
                              T.a(href='oops')[
                                'This link causes an exception!'],

                                ]]
                                  )

        def locateChild(self, ctx, segments):
            ctx.remember(The404Page(), inevow.ICanHandleNotFound)
            ctx.remember(The500Page(), inevow.ICanHandleException)
            return rend.Page.locateChild(self, ctx, segments)

    root = RootPage()
    root.putChild('oops', OopsPage())

    site = appserver.NevowSite(root)
    # You should be able to do this instead of in locateChild, but there seems to be a bug
    # http://divmod.org/trac/ticket/526
    #site.remember(The404Page(), inevow.ICanHandleNotFound)
    #site.remember(The500Page(), inevow.ICanHandleException)

    application = service.Application('News item editor')
    strports.service('8080', site).setServiceParent(application)
