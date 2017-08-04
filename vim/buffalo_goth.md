# Buffalo + Goth

from: http://gobuffalo.io/docs/examples#using-goth-with-buffalo

## generate provider
```
buffalo g goth <provider>
```
## example auth callback 
``` go
package actions

import (
	"fmt"
	"os"

	"github.com/gobuffalo/buffalo"
	"github.com/gobuffalo/gothrecipe/models"
	"github.com/markbates/goth"
	"github.com/markbates/goth/gothic"
	"github.com/markbates/goth/providers/github"
	"github.com/markbates/pop"
	"github.com/pkg/errors"
)

func init() {
	// only needed for lack of vendoring
	gothic.Store = App().SessionStore

	goth.UseProviders(
		github.New(os.Getenv("GITHUB_KEY"), os.Getenv("GITHUB_SECRET"), fmt.Sprintf("%s%s", App().Host, "/auth/github/callback")),
	)
}

func AuthCallback(c buffalo.Context) error {
	gu, err := gothic.CompleteUserAuth(c.Response(), c.Request())
	if err != nil {
		return c.Error(401, err)
	}
	tx := c.Value("tx").(*pop.Connection)
	q := tx.Where("provider = ? and provider_id = ?", gu.Provider, gu.UserID)
	exists, err := q.Exists("users")
	if err != nil {
		return errors.WithStack(err)
	}
	u := &models.User{}
	if exists {
		err = q.First(u)
		if err != nil {
			return errors.WithStack(err)
		}
	} else {
		u.Name = gu.Name
		u.Provider = gu.Provider
		u.ProviderID = gu.UserID
		err = tx.Save(u)
		if err != nil {
			return errors.WithStack(err)
		}
	}

	c.Session().Set("current_user_id", u.ID)
	err = c.Session().Save()
	if err != nil {
		return errors.WithStack(err)
	}

	c.Flash().Add("success", "You have been logged in")
	return c.Redirect(302, "/")
}
```
## example logout func
``` go
func AuthDestroy(c buffalo.Context) error {
	c.Session().Clear()
	err := c.Session().Save()
	if err != nil {
		return errors.WithStack(err)
	}
	c.Flash().Add("success", "You have been logged out")
	return c.Redirect(302, "/")
}
```
## example middleware to set user in the Context
``` go
func SetCurrentUser(next buffalo.Handler) buffalo.Handler {
	return func(c buffalo.Context) error {
		if uid := c.Session().Get("current_user_id"); uid != nil {
			u := &models.User{}
			tx := c.Value("tx").(*pop.Connection)
			err := tx.Find(u, uid)
			if err != nil {
				return errors.WithStack(err)
			}
			c.Set("current_user", u)
		}
		return next(c)
	}
}
```
## example of simple authorization middleware
``` go
func Authorize(next buffalo.Handler) buffalo.Handler {
	return func(c buffalo.Context) error {
		if uid := c.Session().Get("current_user_id"); uid == nil {
			c.Flash().Add("danger", "You must be authorized to see that page")
			return c.Redirect(302, "/")
		}
		return next(c)
	}
}
```
## auth code in app.go
``` go
auth := app.Group("/auth")
		bah := buffalo.WrapHandlerFunc(gothic.BeginAuthHandler)
		auth.GET("/{provider}", bah)
		auth.GET("/{provider}/callback", AuthCallback)
		auth.DELETE("", AuthDestroy)
auth.Middleware.Skip(Authorize, bah, AuthCallback)
```
